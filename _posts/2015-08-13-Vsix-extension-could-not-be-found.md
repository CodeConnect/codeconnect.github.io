---
layout: post
title: Fixing "Extension could not be found" error
---

Occasionally when working on [Code Connect](http://codeconnect.io/) and [Alive](http://comealive.io/) Visual Studio would cease to build our VSIX package. The error we would see said `Extension '<GUID>' could not be found. Please make sure the extension has been installed.`

![screenshot of the error](/images/Vsix-extension-could-not-be-found/ExtensionCouldNotBeFound.png)

We found a few solutions online but none of them worked for us. Eventually we worked out a very simple solution:

### Solving Extension could not be found

To get Visual Studio to build the solution, open the extension's main project's **properties**. Navigate into **Debug** tab. Under **Start Options**, clear **Command line arguments**

![video screenshot of the solution](/images/Vsix-extension-could-not-be-found/solution.gif)

Hit `Ctrl+Shift+B` to build the solution. Once it builds, restore **Command line arguments** to `/rootsuffix Exp` (or anything that was there previously) and build again.

This time the package will be built and the extension will be deployed to the Experimental Instance of Visual Studio.

### Update: Another solution

*This section was added on 2016.01.14*

The above fix doesn't always work. [Zpqrtbnk pointed out](https://github.com/zpqrtbnk/Zbu.ModelsBuilder/issues/52#issuecomment-171639289) that the error can be corrected by manually launching and closing the experimental instance of Visual Studio.

![start experimental instance](/images/Vsix-extension-could-not-be-found/manualStart.png)

It looks like closing the experimental instance fixes its state. After power-cycling VS like this, you should be able to build and deploy your package.

### Other solutions

Other solutions that we've found, but didn't *always* work for us

 - Increasing the version of the package in the .vsixmanifest file
 - Running **Reset the Visual Studio 2015 Experimental Instance** tool
 - Uninstalling extension from the Visual Studio Experimental Instance

Good luck!

Follow us on Twitter: [@CodeConnectHQ](https://twitter.com/CodeConnectHQ)
