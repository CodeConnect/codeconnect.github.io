---
layout: post
title: Alive v1.0 Released!
---

It's been almost six months since we first announced [Alive](http://comealive.io) and we've been working hard building out features and fixing bugs. Today we're excited to announce Alive v1.0 and with this release we're please to offer a **free 30 day trial**.

To celebrate, I thought it'd be fun to take a look at how far we've come and how we got here.

#### Alive on May 15, 2015

<video autoplay loop width="640" height="250" preload>
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/oldalive.mp4" type="video/mp4">
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/oldalive.webm" type="video/webm">
</video>


#### Alive Today

<video autoplay loop width="640" height="250" preload>
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/alivenw.mp4" type="video/mp4">
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/alivenew.webm" type="video/webm">
</video>


It turns out just running the user's tests is a challenge in itself. Our initial implementation was naive and simply looked for methods with `[TestMethod]` or `[Fact]` attributes. It didn't respect the semantics of each test framework and was there only to offer a proof of concept. In v1.0 we've completely overhauled our test runners to respect properties like `[TestCleanup]` in NUnit and `IClassFixture<T>` in xUnit. 

Alive originally bulk ran test cases from within the `devenv.exe` process. Suffice it to say this was not a good design choice. When you kick off a ten minute test run that's allocating objects, you're going to get quite a few visits from the garbage collector which happily freezes all the threads within `devenv.exe`. 

Which leads to this:

![image](http://i.imgur.com/3lcRlkF.png)

Which leads to this:

![image](http://i.imgur.com/wRtn5wy.gif)

We've since made the filtering of test cases option and moved our bulk test runner out of process where it can't directly cause any harm to Visual Studio.

## New Features

You can run a method and then jump to called methods and use Alive on them, while still kicking off execution from the original method.

<video autoplay loop width="640" height="250" preload>
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/alive_continue.mp4" type="video/mp4">
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/alive_continue.webm" type="video/webm">
</video>


You can now run test methods directly, making it easy to start new sessions of Alive quickly.

<video autoplay loop width="640" height="250" preload>
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/docs/TestMethod.mp4" type="video/mp4">
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/docs/TestMethod.webm" type="video/webm">
</video>


## Still work to be done

The Alive Team still has a lot of work to be done. Performance isn't where we need it to be when working with large projects like EntityFramework and Roslyn and a number of known bugs remain unsquashed. We've also heard requests for support to be extended to Javascript, Java, Python and VB.NET. While we don't have anything concrete to announce, we are planning to support a second language over the course of 2016.

One final note is that we're hoping to bring support to DNX projects in the next couple months. As the new ASP.NET stabilizes, it should become an easier target for us to reach. 



