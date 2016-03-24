---
layout: post
title: Tracing code execution with Alive
---

We built [Alive](http://comealive.io) with one goal in mind: to show you what your code is doing the moment you write it. Recently, one of our users [let us know](https://github.com/CodeConnect/AliveFeedback/issues/44) that they wanted to see which lines of code were executed in their current function. 

While Alive largely focuses on showing you what data flows through your programs, we saw this as an opportunity to visualize control flow as well.


Today we're happy to announce the introduction of **code highlighting** for Alive. Code highlighting grays out lines of code that were not executed, making it even easier to understand your code. 

![GIF that shows before and after the highlighting](/images/Highlighting/highlighting.gif)

Highlighting makes it easy to see which code paths were taken in a method and how these paths are impacted when you change your code.

<video autoplay loop preload>
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/highlighting-video.mp4" type="video/mp4">
		<source src="https://codeconnectcdn.blob.core.windows.net/cdn/blog/2016.02.02/highlighting-video.webm" type="video/webm">
</video>

Highlighting reveals where the assertion aborted code execution.

![Screenshot that shows behavior with exception](/images/Highlighting/highlighting-with-exception.png)


As usual, we're always open to feature requests and bug reports on our [public issue tracker](http://github.com/CodeConnect/AliveFeedback/). 


P.S. We’re interested in investigating how Alive could also be used to teach students how to program. If you run a code camp or teach classes on programming please shoot us an email at: founders@comealive.io for some free sponsored licenses!



**Like what you see?**

Why not start a 30-day trial of Alive? Download Alive right from the [Visual Studio Gallery](https://visualstudiogallery.msdn.microsoft.com/4af8eb1a-c64f-4da8-9bf0-6835cf3e95c8).

Follow us on Twitter: [@CodeConnectHQ](https://twitter.com/codeconnecthq)
