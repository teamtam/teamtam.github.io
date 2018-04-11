---
title:  5 VSTS Hosted Mac Agent Tips For Xamarin
description:  "Tips to get your VSTS build pipeline running quickly for your Xamarin apps"
author: Benjamin Tam
categories: Mobile
tags: [VSTS, Xamarin]
comments: true
---

I had been eagerly waiting for a chance to try out the new VSTS Hosted Mac Agents, which were released in preview last November. When the Mac Mini at my client started becoming more of an unreliable liability, I happily took the opportunity. Shortly after a demo where I created a build for my client's Xamarin app in VSTS, the necessary stakeholders approved and linked it to an existing Azure subscription for billing. It was an easy sell to those who had the responsiblity of managing the old box!

If you have a new Xamarin app, then chances are the available Xamarin build definition templates will just work with minimal configuration. However, if there is a legacy app involved there could some minor speed bumps. Accordingly, I thought I would document some of the nuances and lessons learned along the way.

### 1. Xamarin.Android Solution Path
Out of the box, the Xamarin.Android build definition template uses a `\` (back slash) in the file matching pattern to find the solution(s). This will need to change this to a `/` (forward slash), which is of course the standard delimiter on macOS. Otherwise, the default build definition will yield a false positive (i.e. green) at the NuGet restore task, but then fail at the subsequent build task as the required packages won't be restored.

### 2. Xamarin.iOS Build Tool
The Xamarin.iOS build task uses xbuild for the build tool as standard. However, xbuild has been deprecated in favour of MSBuild as of of [Mono 5.0.0](http://www.mono-project.com/docs/about-mono/releases/5.0.0/#msbuild).

### 3. NuGet Restore
The Xamarin.iOS build task will restore NuGet packages by default. If there are packages which don't come from NuGet,  additional steps are required to install NuGet and restore packages. Conversely, the Xamarin.Android build definition template already does this.

Note that if there are packages from NuGet and one additional source, this cannot be configured simply through the UI by entering a value in _Use packages from this VSTS/TFS feed_ and then selecting _Use packages from NuGet.org_. This must be done through a [NuGet.Config file](https://docs.microsoft.com/en-us/nuget/reference/nuget-config-file).

### 4. App Metadata
To automatically bump the version of the app or change the package name as part of the build, have a look at [James Montemagno's task extensions](https://montemagno.com/introducing-vsts-mobile-build-tasks-extension/).

Note that while the name of the extension is _Mobile App Tasks for iOS and Android_, it actually comprises of 4 different tasks. After installing the extension, it is the name of the individual tasks that need to be added to the build.

### 5. One Build For Both iOS & Android
Currently, VSTS provide separate build definition templates for Xamarin.iOS and Xamarin.Android. You can of course have separate builds for each platform. However, I find that with Xamarin projects, it is often easier for both the developer(s) and other stakeholders to keep builds and app versions in lockstep across all platforms. My preferred approach to achieve this is to just create my own custom build definition for both platforms.
