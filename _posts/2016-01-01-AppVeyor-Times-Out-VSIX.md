---
layout: post
title: Continuous Integration times out when building VSIX project
---

Recently our AppVeyor builds started to intermittently fail with message **Build execution time has reached the maximum allowed time for your plan (60 minutes).** We fixed it with a one line change to the .csproj

If the CI build succeeds and all tests pass, yet the process times out, check the CI console for the following line:

```
Setting up Visual Studio for debugging extensions. This one-time operation may take a minute or more.
```

This operation takes over half an hour in our setup and eventually times out the CI build. We fixed this issue by preventing VSIX deployment to Visual Studio during CI builds.

### Solution: conditionally prevent deploying the extension 

To prevent deploying the extension in AppVeyor builds, use the following element in your Visual Studio Extension (VSIX) project's .csproj file:

```xml
<DeployExtension Condition="'$(AppVeyor)' != ''">False</DeployExtension>
```

In the extension's .csproj find all instances of `<DeployExtension>True</DeployExtension>` and modify them to match the code above. 

The .csproj may not have the element `DeployExtension` since it defaults to `True`. In this case, you should add this element either to the first `<PropertyGroup>` or to one of configuration-specific property groups like 

```xml
<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|AnyCPU' ">
```

We added it to the first `<PropertyGroup>`: 

![fix](https://i.gyazo.com/34827523bc0d98741a95d182ffc525d8.png)

Credit for this solution goes to **latkin**'s [PR to the Microsoft/visualfsharp repo](https://github.com/Microsoft/visualfsharp/pull/301/files). 

***

You might have also noticed many compilation messages at the very bottom of the log that mentiont `PNPDTest`. I'm not sure if they're related to the issue (they could be the reason of the delay), but scrolling up above them will reveal the `Setting up Visual Studio for debugging extensions` line.

```
DEVENV : warning : Test name: PNPDTest::PNPCancelStopDevice - Unique property value already defined.  This value will be overwritten and may cause unexpected behaviors.  Property: Kits.Description.  Old value: This test exercises various Plug and Play (PnP)-related code paths in the driver and user-mode components..  New Value: This test uses SetupDi API to send DIF_REMOVE request for the installers to process remove..  Current property level: Test.
```
