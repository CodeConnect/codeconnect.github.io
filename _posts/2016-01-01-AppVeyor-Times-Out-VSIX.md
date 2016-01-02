---
layout: post
title: Continous Integration times out when building VSIX project
---

Recently our AppVeyor builds of Alive started to intermittently fail with message **Build execution time has reached the maximum allowed time for your plan (60 minutes).** We fixed it with a one line change to the .csproj

If the CI build succeeds and all tests pass, yet the process times out, check the CI console for the following line:

```
Setting up Visual Studio for debugging extensions. This one-time operation may take a minute or more.
```

If you see it, you have a chance of fixing time timeouts by preventing VSIX deployment to Visual Studio on CI builds AppVeyor. 

The following solution fixes the issue in AppVeyor, but it can be altered to work with other CI providers:

#### Solution: conditionally prevent deploying the extension 

To prevent deploying the extension in AppVeyor builds, use the following attribute in your Visual Studio Extension (VSIX) project's .csproj file:

```xml
<DeployExtension Condition="'$(AppVeyor)' != ''">False</DeployExtension>
```

In the extension's .csproj find all instances of 
```xml
<DeployExtension>True</DeployExtension>
``` 
and modify them to match the code above. 

The .csproj may not have attribute `DeployExtension`, since its default value is `True`. In this case, you should add these attributes to build configuration. Either add them to the first `<PropertyGroup>` or to one of configuration-specific property groups like 

```xml
<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|AnyCPU' ">
```

We love pictures, too. Here's the screenshot of the fix: ![fix](https://i.gyazo.com/34827523bc0d98741a95d182ffc525d8.png)

Credit for this solution goes to **latkin**'s [PR to the Microsoft/visualfsharp repo](https://github.com/Microsoft/visualfsharp/pull/301/files)

~~~

You might have also noticed many compilation messages at the very bottom of the log that mentiont `PNPDTest`. I'm not sure if they're related to the issue (they could be the reason of the delay), but scrolling up above them will reveal `Setting up Visual Studio for debugging extensions`.

```
DEVENV : warning : Test name: PNPDTest::PNPCancelStopDevice - Unique property value already defined.  This value will be overwritten and may cause unexpected behaviors.  Property: Kits.Description.  Old value: This test exercises various Plug and Play (PnP)-related code paths in the driver and user-mode components..  New Value: This test uses SetupDi API to send DIF_REMOVE request for the installers to process remove..  Current property level: Test.
```
