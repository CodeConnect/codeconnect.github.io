---
layout: post
title: UI Throwback Thursday
---

With [Alive](//comealive.io/) scheduled to be released as v1.0 on October 26th (psst, that's when the discount for early adopters ends!) I wanted to look back in time at Alive, as it grew from a toy and into a tool. Our early adopters played a huge role in the evolution of Alive's user interface. Many actively requested features or new behaviors while others helped shape it by discussing their pain points and areas where Alive could be improved.

![screenshot](/images/UI-Throwback-Thursday/v06ui.png)

---

# Alpha, v0.2

We announced Alive in May and decuded we would release Alive publicly on on June 1st. The first user interface was basic and minimal. A simple icon let the user start Alive on the current method, but didn't provide much feedback. Users had no way of knowing whether their code was currently compiling, running or had halted due to an error.

<video width="554" height="158" autoplay loop>
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v02basic.webm" type="video/webm">
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v02basic.mp4" type="video/mp4">
*Video of interface in version 0.2. (your browser does not support the video tag)*
</video>

---

# Beta, v0.5

For our transition from alpha to beta we completely overhauled the user interface. The dominant theme is this rewrite was to create an icon that told the user what was going on. The user should be able to quickly understand when Alive is working, when it's completed successfully or when it's encountered an error.

<video width="556" height="292" autoplay loop>
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v05button.webm" type="video/webm">
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v05button.mp4" type="video/mp4">
*Video of interface transitions in version 0.5. (your browser does not support the video tag)*
</video>
We rebuilt the icon to be transparent and to match the user's Visual Studio theme. Create it as a XAML user control meant that it could scale to high resolution displays and still remain crisp and clean.

<video width="554" height="158" autoplay loop>
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v05test selection.webm" type="video/webm">
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v05test selection.mp4" type="video/mp4">
*Video of interface in version 0.5. (your browser does not support the video tag)*
</video>

Our early adopters wanted to use Alive without being forced to first run all their tests. (Some had test suites that took over an hour to complete!) The process of matching tests with methods they invoke became optional and could be triggered by clicking "Autofilter". Simply finding a list of tests in a project is very quick, so we decided to let the user pick a test case from the list of all tests.

---

# Almost ready, v0.6

The next task was to reduce the number of clicks it took to use Alive. We added two fly-out buttons that launch Alive in specified mode, replacing the tabs found in the previos version.

<video width="554" height="158" autoplay loop>
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v06launch test.webm" type="video/webm">
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v06launch test.mp4" type="video/mp4">
*Video of interface in version 0.6. (your browser does not support the video tag)*
</video>

Alive learns how you use it, so that you can just press the main button to get started. Keyboard shortcut `Ctrl+[, Ctrl+[` will also start Alive. All shortcuts that we had our eye on were taken :(

<video width="554" height="158" autoplay loop>
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v06smart launch and keystroke.webm" type="video/webm">
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v05test selection.mp4" type="video/mp4">
*Video of interface enhancement in version 0.6. (your browser does not support the video tag)*
</video>

Following suggestions from our Early Adopters, Alive now uses a small fly-out icon to explain why a test case failed, and if there were any unhandled exception.

<video width="554" height="158" autoplay loop>
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v06test failed message.webm" type="video/webm">
  <source src="{{baseurl}}/images/UI-Throwback-Thursday/v06test failed message.mp4" type="video/mp4">
*Video of interface with a failing test case in version 0.6. (your browser does not support the video tag)*
</video>

And last but not least, aligning code execution results to the right made them more legible.

---

[Multiply your productivity with Alive](//comealive.io/)

Follow us on Twitter: [@CodeConnectHQ](https://twitter.com/CodeConnectHQ)
