---
layout: post
title: Three Months of Alive
---


Just three months ago we released [Alive](http://comealive.io) v1.0 to the public. Since then we’ve been working hard to address your feedback and continue to improve Alive. In total, we published 12 releases with a focus on improved user experience, better performance and expanding the type of code Alive can run.

### UX: Drilldown

Alive had some serious drawbacks when it came to working with complex datatypes. In order to inspect properties of an object, we’d have to create intermediate variables and wait for Alive to re-run our code.

<video autoplay loop preload>
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/2016.02.02/DrillDown.mp4" type="video/mp4">
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/2016.02.02/DrillDown.webm" type="video/webm">
</video>

Drilldown was a long-awaited feature for many users. It allows us to easily inspect the first level of properties within an object.

### UX: Icon

Over the last few months we reached out to users via interviews, surveys and our public issue tracker. We quickly realized that one of the biggest hurdles facing our users was that they weren’t sure when they could use Alive. To combat this, we’ve redesigned our icons to better convey when Alive can be launched directly on a method and when you’ll have to instrument a method via a unit test.

**Play icon - You can launch this method directly**

<video autoplay loop preload>
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/2016.02.02/PlayIcon.mp4" type="video/mp4">
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/2016.02.02/PlayIcon.webm" type="video/webm">
</video>

**Test icon - You’ll have to launch this method via test case**

<video autoplay loop preload>
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/2016.02.02/TestIcon.mp4" type="video/mp4">
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/2016.02.02/TestIcon.webm" type="video/webm">
</video>

**Error icon - There was a warning, error or exception when running your code. Simply over over this icon to see the root cause**

<video autoplay loop preload>
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/2016.02.02/WarningIcon.mp4" type="video/mp4">
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/2016.02.02/WarningIcon.webm" type="video/webm">
</video>

## Performance

As we hurried to ensure Alive met all the functional goals we laid out for it in time for our release, we ran out of time to properly focus on Alive’s performance. In November, if you used Alive on Newtonsoft.Json you might have seen up to 14 seconds (yikes!) between subsequent runs of Alive. Immediately after our launch we took steps to drastically improve performance.

### Performance: Caching

Alive runs your code after compiling and emitting it to a directory in %LocalAppData%. This means we have to copy all referenced DLLs to the directory before running your code. The first step we took was to stop any unnecessary copies of DLLs to this directory. If an assembly hasn’t changed between subsequent runs of Alive, we shouldn’t waste time copying it around.

This was true no only for referenced DLLs, but for DLLs generated when building your projects. In fact, if a project hasn’t changed we shouldn’t even waste time compiling it in the first place and instead cache the DLL.

### Performance: Emit To Memory

Next, we realized that we could save a healthy chunk of time if we avoided emitting frequently changing DLLs to disk. Instead we’d compile and emit them directly to memory. This saved us a lot of unnecessary I/O overhead and improved performance by about 30%.

### Instance Methods

Another common piece of feedback we heard from users was:

> I don’t write tests, can I still use Alive?

In November we didn’t really have a good answer to this. Alive v1.0 was only able to run static methods and unit tests, which made it difficult to use Alive on untested code. We’ve since done a lot of work to increase Alive’s reach. Today we’ve extended support to instance methods if we are able to create the parent type.

<video autoplay loop preload>
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/2016.02.02/InstanceInvocation.mp4" type="video/mp4">
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/2016.02.02/InstanceInvocation.webm" type="video/webm">
</video>

### Entity Framework and Embedded Resources

Alive has had a known bug (since October!) that affects code that interacts with Entity Framework. The underlying reason for this is that EF uses a custom built task to embed special CSDL, MSL and SSDL resources within the output DLL. It turns out we had a similar problem embedding `.resx` resource files within output DLLs. We’re happy to announce that we’ve fixed both these issues and Alive should work find with both Entity Framework and embedded resources!
