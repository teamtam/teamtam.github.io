---
title: ASP.NET Core Environment Variables In Octopus
description:  "Octopus Deploy Step Template to set environment variables for ASP.NET Core"
author: Benjamin Tam
categories: DotNetCore
tags: [Tools, Octopus, PowerShell]
comments: true
---

Inspired on a project during which I migrated a legacy ASP.NET MVC app to Core, I recently made a small contribution to the [Octopus Deploy Community Library](https://library.octopus.com/listing). I added some polish and testing to a PowerShell script I had written that sets an environment variable through web.config for ASP.NET Core projects, and then the kind folk at Octopus approved my pull request.

The most likely use for this step template is to take advantage of the MVC [environment tag helpers](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/environments) now available in ASP.NET Core.

The benefit of setting an environment variable in web.config rather than other possible places is that not only will the scope be restricted to the runtime of the app, it will also take precedence over corresponding environment variables configured in IIS, user environment variables or system environment variables. This means it is easier to independently configure apps deployed on the same machine.

The need arose partly due to web.config transforms no longer being available in ASP.NET Core. I believe Microsoft took the correct approach with this since environment specific configuration should not be part of the source code or even the build, but where appropriate should be part of the deployment process. Another possible approach to the problem is to use token substitution to manage environment variables in web.config. However, doing so will likely break your local configuration.

Finally, I have written the PowerShell script so that it can either run by itself or inside the execution context of a Tentacle agent. This of course means the same script could be used in other deployment tools. Loosely on my _to-do_ list is to create a corresponding task extension for VSTS.

## Links

 * [Step Template](https://library.octopus.com/step-templates/c7f96ab8-a0d3-4f01-928e-c8cb78ab108c/)
 * [Source Code](https://github.com/teamtam/octopus-templates)
