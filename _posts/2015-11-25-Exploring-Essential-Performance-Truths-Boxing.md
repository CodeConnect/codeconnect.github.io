---
layout: post
title: Exploring Essential Performance Truths - Boxing
---

In [Part 1](http://blog.comealive.io/Exploring-Essential-Performance-Truths/), we discussed four facts about performance in a large, user-facing, managed codebase. Ultimately most fixable bottlenecks in these systems come down to allocations and the garbage collector (GC). Today we'll look at a common source of wasteful allocations: Boxing.

#### Value Types and Reference Types

In order to understand boxing, it's important to understand a little bit about reference types and value types in C#. In general:

 - Value types (`struct`, `enum`, `bool`, `int`, `float` etc.) are allocated on the stack.
 - Reference types (`class`, `interface`, `arrays`, `string` etc.) are allocated on the heap.

Some argue this is just an [implementation detail](http://blogs.msdn.com/b/ericlippert/archive/2009/04/27/the-stack-is-an-implementation-detail.aspx), but from a performance perspective, it's an implementation detail we must understand. The stack is pre-allocated for us, so when we place a new integer value on it we don't run the risk of triggering GC. In contrast, the heap is guarded closely by the the GC. As objects are allocated on the heap, the garbage collector periodically reacts and takes time to free memory that's no longer being used. A single heap allocation runs the risk of triggering an expensive garbage collection operation. We've previously established this as a decidedly **BadThing™** that we'd like to avoid.

#### Boxing

Boxing is defined by MSDN as:

> Boxing is the process of converting a value type to the type object or to any interface type implemented by this value type.

What does this look like? The simplest example:

```CSharp
int ourInt = 5;           //A 4 byte stack allocated integer
object ourObject = 5;     //Boxing a 4 byte integer into a 12 byte heap allocated object
```

Note that we've not only allocated on the heap, but we're using three times as much memory to store our `int`. As all objects are at least 12 bytes, the overhead can be even more extreme when boxing `short` (2 bytes) or `byte` (1 byte).

So how can we measure and detect when boxing is taking place? One way is to use Visual Studio's Diagnostic tools to take a heap snapshot and then look for values like `Int32` or `Char` on the heap. You can click on them and see how they were allocated. Here we box a bunch of local variables:

<video autoplay loop preload height="300" width="717">
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/BoxingInts.mp4" type="video/mp4">
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/BoxingInts.webm" type="video/webm">
</video>

#### Real-World Example #1

Let's jump into some real-world examples discovered while building Roslyn. Can you see what's wrong with the following code?

```CSharp
int id = 5;
int size = 100;
string result = String.Format("{0}:{1}", id, size);
```

The problem is a little clearer when we hover over `String.Format` and Visual Studio shows us the overload we're using.

![image](https://cloud.githubusercontent.com/assets/1249087/11408844/bfc72086-9389-11e5-9a3e-72a8962c1d7b.png)

The issue isn't immediately clear without Visual Studio, so similar code likely passes code reviews every day. The problem here is that `String.Format` accepts two objects as parameters and we're passing in integer. This means our two integers get boxed and thus heap-allocated. :(

Luckily, the solution is pretty simple:

```CSharp
int id = 5;
int size = 100;
string result = String.Format("{0}:{1}", id.ToString(), size.ToString());   //Call .ToString() to avoid boxing.
```

"But wait!" you say, "Aren't we just allocating strings here now?".

It's true, but `String.Format()` would have ended [up calling `.ToString()` internally](http://referencesource.microsoft.com/#mscorlib/system/text/stringbuilder.cs,1466) regardless, so we're not introducing any new allocations here. Our fix has allowed us to avoid the two unnecessary boxing allocations, but the allocations caused by `.ToString()` cannot be avoided.

#### Real-World Example #2

It turns out these string functions are filled with little gotcha's like the above, so it's important to consider the overload you'll end up using. Sometimes, it's even less visible. Do you see what might cause boxing in the following?

```CSharp
int id = 0;
int size = 10;
string result = id.ToString() + ':' + size.ToString();
```

The third line is essentially equivalent to simply writing:

```CSharp
string result = String.Concat(id.ToString(), ':', size.ToString());
```

The problem here is that there is no overload that accepts `(string, char, string)` so the compiler resolves to `(object, object, object)`. The net result here is that `char` (a value type) gets boxed. We can solve this by concatenating three strings together instead:

```CSharp
string result = id.ToString() + ":" + size.ToString();
```

This small change doesn't really compromise readability, so it's probably worth applying broadly to your codebase where applicable. 

#### Real World Example #3

Sometimes boxing is out of your hands entirely and the only way you can catch it is by measuring. The following example is one of those cases:

```CSharp
public enum Color { Red, Green, Blue }
public class BoxingExample
{
  private string _name;
  private Color _color;

  public override int GetHashCode()
  {
     int stringHashCode = _name.GetHashCode();          // No allocations
     int enumHashCode = _color.GetHashCode();           // 1 allocation :(
     int intHashCode = ((int)_color).GetHashCode();     // No allocations
     return stringHashCode ^ enumHashCode;
  }
}
```

It turns out the internal implementation of `enum.GetHashCode()` boxes. In Bill Chiles' original document, he said:

>This problem is very subtle. PerfView would report this as `enum.GetHashCode()` boxing because for implementation reasons, this method boxes the underlying representation of the enumeration type. If you look closely in PerfView, you may see two boxing allocations for each call to `GetHashCode()`. The compiler inserts one, and the .NET Framework inserts the other.

So he doesn't specify exactly why the boxing takes place beyond "implementation reasons". These days, we can [view the source](http://referencesource.microsoft.com/#mscorlib/system/enum.cs,f527a799d76cc18a) code ourselves. I did that and… I honestly still had no idea why it's boxing. So if you understand, please leave a comment and I'll add it below:

<center>
![image](https://cloud.githubusercontent.com/assets/1249087/11408515/a731f39a-9387-11e5-96d0-886141b45f17.png)
</center>

You may have noticed Bill suggested there should be two boxing allocations, but in my tests I noticed only one. In the source code, I see a comment that says:

```CSharp
  // Avoid boxing by inlining GetValue()
  // return GetValue().GetHashCode();
```

So perhaps one of the boxing operations was removed? Perhaps I can't profile correctly? I'm not sure, but I do know that I'll keep an eye out for any calls to `enum.GetHashCode()` or for any `Dictionary` that uses an `enum` as it's key.

By the way, the fix for this problem is to cast the `enum` to `int` (or appropriate base type) before calling `GetHashCode()`.

```CSharp
int intHashCode = ((int)_color).GetHashCode();      //All good in the hood
```

#### Conclusion

Boxing is one of the cheaper performance bottlenecks that you might find yourself fixing. Cleaning up method invocations and tweaking parameters is not a heavy price to pay for the allocation savings that you might see from this. In fact, not a single one of these examples required you to add a new line of code.

In the first chapter of this series, we discussed the fact that you shouldn't optimize prematurely. I might go so far as to say that in simple cases like these, I'd probably just opt out of boxing once I was aware of it. Code readability doesn't suffer and our code doesn't become more complex because our changes. All great things and easy wins in my book.








