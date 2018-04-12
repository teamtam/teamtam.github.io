---
title:  5 VSTS Hosted Mac Agent Tips For Xamarin
description:  "Tips to get your VSTS build pipeline running quickly for your Xamarin apps"
author: Benjamin Tam
categories: Mobile
tags: [VSTS, Xamarin]
comments: true
---

I had been eagerly waiting for a chance to try out the new VSTS Hosted Mac Agents, which were [released in preview last November](https://blogs.msdn.microsoft.com/devops/2017/11/16/cloud-hosted-mac-agents-for-ci-cd-pipelines/). I happily took the opportunity to do so when the Mac Mini build server at my client started becoming more of an unreliable liability. Shortly after a demo where I created a build pipeline for my client's Xamarin app in VSTS, the necessary stakeholders approved and linked it to their existing Azure subscription for billing. Sales isn't one of my core skills, but this was an easy sell!

If you have a new Xamarin app, then the build definition templates available with VSTS should work with minimal additional configuration. However, if there are pre-established processes involved, there could some minor speed bumps. Accordingly, I thought I would document some of the nuances and lessons learned along the way.

### 1. Xamarin.Android Solution Path
Out of the box, the Xamarin.Android build definition template uses a `\` (back slash) in the file matching pattern to find the solution(s). This will need to change this to a `/` (forward slash) when using a Mac build agent, which is of course the standard delimiter on macOS. Otherwise, the default build definition will yield a false positive (i.e. green) at the NuGet restore task, but then fail at the subsequent build task as the required packages won't be restored.

![Change the back slash to a forward slash](/assets/5-vsts-mac-agent-tips/AndroidSolutionPath.png)

### 2. Xamarin.iOS Build Tool
The Xamarin.iOS build task uses xbuild for the build tool as standard. However, xbuild has been deprecated in favour of MSBuild as of [Mono 5.0.0](http://www.mono-project.com/docs/about-mono/releases/5.0.0/#msbuild).

![Use MSBuild instead of xbuild](/assets/5-vsts-mac-agent-tips/IosBuildTool.png)

### 3. NuGet Restore
The Xamarin.iOS build task will restore NuGet packages by default. If there are packages which don't come from NuGet,  additional steps are required to first install NuGet itself before restoring packages. Conversely, the Xamarin.Android build definition template already does this.

Note that if there are packages from NuGet and one additional source, this cannot be configured simply through the UI by entering a value in _Use packages from this VSTS/TFS feed_ and then selecting _Use packages from NuGet.org_. This must be done through a [NuGet.Config file](https://docs.microsoft.com/en-us/nuget/reference/nuget-config-file).

![Use separate NuGet build tasks with NuGet.Config for additional packages](/assets/5-vsts-mac-agent-tips/NuGetRestore.png)

### 4. App Metadata
To automatically bump the version of the app or change the package name as part of the build, have a look at [James Montemagno's VSTS extensions](https://marketplace.visualstudio.com/items?itemName=vs-publisher-473885.motz-mobile-buildtasks).

Note that while the name of the extension in the VSTS Marketplace is _Mobile App Tasks for iOS and Android_, it actually comprises of 4 different tasks. After installing the extension, it is the name of the individual tasks that can be added to the build.

![Use Mobile App Tasks for iOS and Android to change app metadata](/assets/5-vsts-mac-agent-tips/AppMetadata.png)

### 5. One Build For Both iOS & Android
Currently, VSTS provide separate build definition templates for Xamarin.iOS and Xamarin.Android. You can of course have separate builds for each platform. However, I find that with cross-platform mobile projects, it is typically easier for both the developer(s) and other stakeholders to keep builds and app versions in lockstep across all platforms. My preferred approach to achieve this is to just create my own custom build definition for both platforms since a combined build template definition for both Xamarin.Android and Xamarin.iOS is not currently available.

![There are separate build definition templates for Android & iOS but not a combined one](/assets/5-vsts-mac-agent-tips/BuildDefinitionTemplates.png)
