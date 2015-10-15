---
layout: post
title: UI Throwback Thursday
---

With [Alive](//comealive.io/) scheduled to be released as v1.0 on October 26th (psst! that's when the discount for early adopters ends), I wanted to look back in time at Alive as it transformed from a prototype to its current state. Our early adopters played a big role in the evolution of user interface, either actively discussing their pain points, or passively, by being our customers and motivating us to give their tool the perfect shape.

# Alpha, v0.2

We announced Alive in May and set ourselves a milestone of a first public release on June 1st. 
The first user interface was basic and minimal. Access to Alive's functionality was just a mouse move away.

<video width="554" height="158" autoplay loop>
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v02basic.webm" type="video/webm">
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v02basic.mp4" type="video/mp4">
*Video of interface in version 0.2. (your browser does not support the video tag)*
</video>

# Beta, v0.5

The launch icon became less obstrusive when we built it as a transparent, themed vector (XAML) shape. This means that it looks stunning on high resolution displays.

<video width="554" height="158" autoplay loop>
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v05test selection.webm" type="video/webm">
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v05test selection.mp4" type="video/mp4">
*Video of interface in version 0.5. (your browser does not support the video tag)*
</video>

Our Early Adopters wanted to use Alive without the lengthy process of *discovering tests*, which ran all tests and scouted which methods in the code are hit. Discovering tests in the solution itself is very quick, so we let the user pick a test case from the list of all tests. The process of matching tests with methods that they invoke became the *Autofilter*.

# Almost ready, v0.6

The next step was to reduce the number of clicks. We added two fly-out buttons that launch Alive in specified mode, in lieu of a tabs in the previos version...

<video width="554" height="158" autoplay loop>
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v06launch test.webm" type="video/webm">
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v06launch test.mp4" type="video/mp4">
*Video of interface in version 0.5. (your browser does not support the video tag)*
</video>

... Alive learns how you use it, so that you can just press the main button to get started. Keyboard shortcut `Ctrl+[, Ctrl+[` will also start Alive. All shortcuts that we had our eye on were taken :(

<video width="554" height="158" autoplay loop>
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v06smart launch and keystroke.webm" type="video/webm">
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v05test selection.mp4" type="video/mp4">
*Video of interface in version 0.5. (your browser does not support the video tag)*
</video>

Aligning code execution results to the right made them more legible.

<video width="554" height="158" autoplay loop>
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v06test failed message.webm" type="video/webm">
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v06test failed message.mp4" type="video/mp4">
*Video of interface in version 0.5. (your browser does not support the video tag)*
</video>

Following suggestions from our Early Adopters, Alive now uses a small fly-out icon to explain why a test case failed, and if there were any unhandled exception.

---

[Multiply your productivity with Alive](//comealive.io/)

Follow us on Twitter: [@GetCodeConnect](http://twitter.com/GetCodeConnect)
