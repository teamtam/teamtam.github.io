---
id: 5
title: Using Xamarin Android Player Through Parallels
date: 2014-10-26T16:00:50+00:00
author: TeamTam
layout: post
guid: http://teamtam.net/?p=5
permalink: /xamarin-android-player-parallels/
dsq_thread_id:
  - "3521786866"
categories:
  - Mobile
tags:
  - Emulators
  - Xamarin
  - Xamarin Android
---
For Xamarin developers, getting an Android emulator working is&nbsp;a commonly faced challenge since the standard one provided by Google is unworkably slow. When different operating systems and virtual machines are in the equation as well, the potential for encountering problems obviously increases. It is a common scenario however, and is how I prefer to work. More specifically, where possible I develop in Visual Studio inside a Windows virtual machine using Parallels, with Mac OS as the host providing access to the iOS emulator and build host. This allows me to use the IDE I already use on a daily basis when I am not on a mobile project, while also being able to run iOS, Android and Windows Phone emulators all on the one machine.

There is some information out there describing how to get GenyMotion working in such cross-platform environments, which uses the same Oracle VirtualBox technology as the newly launched Xamarin Android Player (XAP) under the covers. However, it is a little inconsistent and often involves some sort of complicated SSH tunnelling. So my grand plan was to document this entire process from end-to-end specifically for XAP since Xamarin did not document this initially. That is &#8230; until I was almost there after doing the crazy SSH tunnelling myself &#8230; I discovered a new <a title="http://motzcod.es/post/100735916227/debug-with-the-xamarin-android-player-from-visual" href="http://motzcod.es/post/100735916227/debug-with-the-xamarin-android-player-from-visual" target="_blank">blog post by Xamarin&#8217;s James Montemagno on the subject!</a>

Newer versions of XAP make it easier still. Now all you need to do is to get the IP Address by clicking on the settings cog wheel in your XAP. In my case, it is 10.71.34.101.
  
[<img class="alignnone size-full wp-image-427" src="http://teamtam.net/wp-content/uploads/2015/03/XamarinAndroidPlayer-IPAddress.png" alt="Xamarin Android Player - IP Address" width="1712" height="1480" srcset="http://teamtam.net/wp-content/uploads/2015/03/XamarinAndroidPlayer-IPAddress.png 1712w, http://teamtam.net/wp-content/uploads/2015/03/XamarinAndroidPlayer-IPAddress-300x259.png 300w, http://teamtam.net/wp-content/uploads/2015/03/XamarinAndroidPlayer-IPAddress-1024x885.png 1024w" sizes="(max-width: 1712px) 100vw, 1712px" />](http://teamtam.net/wp-content/uploads/2015/03/XamarinAndroidPlayer-IPAddress.png)

Then, all you need to do is open an &#8216;_Android Adb Command Prompt&#8217;_ from Visual Studio and enter **adb connect**. You can also enter **adb devices** to check your connection status if you wish.

[<img class="alignnone size-full wp-image-429" src="http://teamtam.net/wp-content/uploads/2015/03/XamarinAndroidPlayer-AdbConnect.png" alt="Xamarin Android Player - adb connect" width="625" height="142" srcset="http://teamtam.net/wp-content/uploads/2015/03/XamarinAndroidPlayer-AdbConnect.png 625w, http://teamtam.net/wp-content/uploads/2015/03/XamarinAndroidPlayer-AdbConnect-300x68.png 300w" sizes="(max-width: 625px) 100vw, 625px" />](http://teamtam.net/wp-content/uploads/2015/03/XamarinAndroidPlayer-AdbConnect.png)

Note that however, you will still need to enable &#8216;_USB debugging mode_&#8216; to allow the Visual Studio debugger to do its thing. For those who don&#8217;t know the secret Android developer handshake &#8230; on your emulated Android device:

1. Go to **Settings**
  
2. Go to **About phone**
  
3. Click on **Build number** _seven times_

Hope that helps! And a big thank you to James and the Xamarin team for saving me from having to do the aforementioned crazy SSH tunnelling! 🙂

**Links**

  * <a title="http://motzcod.es/post/100735916227/debug-with-the-xamarin-android-player-from-visual" href="http://motzcod.es/post/100735916227/debug-with-the-xamarin-android-player-from-visual" target="_blank">http://motzcod.es/post/100735916227/debug-with-the-xamarin-android-player-from-visual</a>

_Updated: 17 March, 2015_