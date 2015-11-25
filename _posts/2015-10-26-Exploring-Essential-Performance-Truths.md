---
layout: post
title: Exploring “Essential Performance Truths”
---

So it turns out when you rewrite two compilers over the better part of a decade, you end up learning a thing or two about performance. Such was the case when the Roslyn team decided to rewrite their compilers for C# and VB .NET in their respective languages.

So far these lessons have manifested themselves in the form of a set of guidelines by Bill Chiles entitled ["Essential Performance Facts and .NET Framework Tips" [PDF]](http://download-codeplex.sec.s-msft.com/Download?ProjectName=roslyn&DownloadId=838017) and a talk by Dustin Campbell entitled ["Essential Truths Everyone Should Know about Performance in a Large Managed Codebase"](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B333).

I wanted to break these lessons apart into a series of posts where we could explore each topic in a little more detail. Where possible I'd also like to approach each topic from that of a beginner without assuming that the readers knows the intricacies of closures, boxing, iterators etc.

I've broken the topic apart into the following sections (which I'll link as I finish them):

- Four Facts About Performance
- Boxing
- String Manipulation
- Iterators
- Delegates and Lambdas
- LINQ
- Collections
- IDisposable
- EventHandler 

### Four Facts About Performance

#### Fact 1: Don't prematurely optimize

One of the fundamental goals of C# is to improve developer productivity. Occasionally this means language features make a trade-off between raw performance and code readability/writability. 

As we go through the above topics, it's important that we actively resist the temptation to apply broad, sweeping changes to our code bases.  Unless poor performance is actively hurting your project and you've identified a bottleneck; **You should always favor program readability over potential optimizations**. Peter Hallam estimates that up to [70% of developer time is spent just reading and understanding code](http://blogs.msdn.com/b/peterhal/archive/2006/01/04/509302.aspx). To combat this, you should go great lengths to write code that is easily understood.

#### Fact 2: If we're not measuring, we're guessing

I don't think many developers would question the above. But based on my own experience, it's easy to say one thing and do another.

It’s not uncommon to see performance related pull requests on GitHub that are unaccompanied by performance testing results that quantify the improvement. Frequently the author is actually correct, and their changes are entirely valid. **But a correct guess is still a guess**. We must measure to find bottlenecks, apply our changes and then measure and quantify our improvements.

#### Fact 3: Good tools make all the difference

This is pretty self-explanatory. In order to measure, we’re going to need tools that can do a good job. A sampling of the tools available:

- [PerfView](http://www.microsoft.com/en-ca/download/details.aspx?id=28567) — Performs well. Powerful. Looks like it was built in the 90s.
- [Visual Studio Diagnostic Tools](http://blogs.msdn.com/b/visualstudioalm/archive/2015/01/16/diagnostic-tools-debugger-window-in-visual-studio-2015.aspx) —  Easy to use. Slow on larger projects.
- [DotTrace](https://www.jetbrains.com/profiler/) — Performs well. Powerful. Costs $$$.

#### Fact 4: It's all about allocations

In university, students usually study algorithms and performance in a world that is CPU bound. We’re taught that every instruction is precious and that we should characterize algorithms using [Big O Notation](https://en.wikipedia.org/wiki/Big_O_notation). When faced with an algorithm that we can’t improve, we consider other approaches such as parellelizing it where possible.

It turns out that in the **RealWorld™** (at least in the context of managed languages) that allocations end up being indirectly responsible for many (most?) bottlenecks.

Garbage collection in .NET can be triggered by a single allocation. Without digressing too much, the GC must occasionally suspend all running threads when it runs. If you’re building user-facing applications this means for a moment in time your UI locks up. And when your UI locks up, users tend to react poorly.

![image](https://cdn-images-1.medium.com/max/1200/1*elLkg4-CzJ_JtE5GdCiL6w.gif)

Avoiding allocations on hot paths can help us avoid these GC pauses and improve our application’s responsiveness.

Finally, it’s worth remembering that these truths and examples come from the Roslyn compiler; a project that runs code on every keystroke. If you’re building web apps, creating batch jobs or building distributed systems then your applications are going to have different performance profiles and entirely different bottlenecks. **Measure, react and then measure again**.
