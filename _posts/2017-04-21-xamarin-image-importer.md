---
id: 135
title: Xamarin Image Importer
date: 2017-04-21T08:00:18+00:00
author: TeamTam
layout: post
guid: http://teamtam.net/?p=135
permalink: /xamarin-image-importer/
hefo_before:
  - "0"
hefo_after:
  - "0"
dsq_thread_id:
  - "5743586111"
categories:
  - Mobile
tags:
  - PowerShell
  - Tools
  - Xamarin
---
Firstly, I have a confession to make &#8230; I am pretty lazy! Perhaps not Homer Simpson lazy, but it&#8217;s something I do aspire to&#8230; 😉 I take some solace from this quote attributed to Bill Gates:

> &#8220;I choose a lazy person to do a hard job. Because a lazy person will find an easy way to do it.&#8221;

On a recent Xamarin.Forms project I was working on, the focus was a UI/UX refresh with a lot of non-linear lines and shapes. Accordingly, there was the need for a lot of images. The requirement was to support @1x, @2x and @3x resolutions on iOS, and mdpi, hdpi, xhdpi and xxhdpi on Android. So for every image required, there were actually 7 variations of the same image at different resolutions. On top of that, as the project went on sometimes images would need to get updated to get things pixel-perfect, while other times churn would be created as the machinations of the business got involved. This added up to many minutes lost to a tedious, error prone, recurring manual task. The pain of this process was particularly felt with Android, which uses the convention of a separate directory for each resolution.

I already had some PowerShell up my sleeve to mitigate some of the pain, but I decided to polish it up and make it available, for the simple joy of writing and shipping code without bureaucracy if nothing else. And so, the <a href="https://github.com/teamtam/xamarin-image-importer" target="_blank" rel="noopener noreferrer">Xamarin Image Importer</a> was born, in the form of a module in the PowerShell Gallery.

This little utility aims to do 2 things only &#8211; (1) copy images to the correct location in the file structure required by iOS / Android (2) add them to .csproj files so they will appear in Visual/Xamarin Studio. Many of the variables of the process can be inferred by convention. This worked well for the project I was working on, where the designer on the team would export images from a SVG using Illustrator then dump it into a Dropbox folder. From there, the PowerShell slurps in the images and does the rest.

While I have no immediate need to develop the tool further for myself right now, some potential additions I can imagine include:

<ol style="padding-left: 20px;">
  <li>
    A pretty simple change to cater for images other than .png
  </li>
  <li>
    Handle project types other than Xamarin e.g. Telerik AppBuilder
  </li>
  <li>
    Take a .svg as the import source, and then do the export into various resolutions automatically, reducing the need for the likes of Illustrator or Sketch purely for this mechanical task (version 2.0?) <ul>
      <li>
        Could perhaps use the <a href="https://github.com/bevelop/logo.gen" target="_blank" rel="noopener noreferrer">logo.gen</a> tool my colleague Poya Manouchehri has been working on
      </li>
    </ul>
  </li>
  
  <li>
    Something else somebody suggests and/or comes up with in a Pull Request
  </li>
</ol>

Anyway, hope this little tool helps some of you out there. Happy lazy coding!

**Links**

  * Xamarin Image Importer
  
    <a href="https://github.com/teamtam/xamarin-image-importer" target="_blank" rel="noopener noreferrer">https://github.com/teamtam/xamarin-image-importer</a>
  * logo.gen
  
    <a href="https://github.com/bevelop/logo.gen" target="_blank" rel="noopener noreferrer">https://github.com/bevelop/logo.gen</a>