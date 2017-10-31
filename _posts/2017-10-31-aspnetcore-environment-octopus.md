---
title: ASP.NET Core Environment Variables In Octopus
description:  "Set environment variables for ASP.NET Core in Octopus Deploy"
author: Benjamin Tam
categories: DotNetCore
tags: [Tools, Octopus, PowerShell]
comments: true
---

Inspired on a project during which I migrated a legacy ASP.NET MVC app to Core, I recently made a small contribution to the [Octopus Deploy Community Library](https://library.octopus.com/listing). I added some polish and testing to an Octopus Step Template I wrote that sets an environment variable through web.config for ASP.NET Core projects, and then the kind folk at Octopus approved my pull request.

The most likely use for this is to take advantage of the MVC [Environment tag helpers](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/environments) now available in ASP.NET Core.

The benefit of setting an environment variable in web.config is that not only will the scope be restricted to the runtime of the app, but it will also take precedence over corresponding ones found in IIS, user environment variables or system environment variables. This means it is easier to independently configure apps deployed on the same machine.

In addition, web.config transforms are no longer an option in ASP.NET Core. If you use a token substitution method to manage environment variables in web.config, it will likely break your local configuration.

## Links

 * [Step Template](https://library.octopus.com/step-templates/c7f96ab8-a0d3-4f01-928e-c8cb78ab108c/)
 * [Source](https://github.com/teamtam/octopus-templates)
