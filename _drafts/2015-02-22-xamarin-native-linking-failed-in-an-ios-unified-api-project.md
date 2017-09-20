---
id: 223
title: Xamarin Native Linking Failed in an iOS Unified API Project
date: 2015-02-22T22:19:58+00:00
author: TeamTam
layout: post
guid: http://teamtam.net/?p=223
permalink: /xamarin-native-linking-failed-in-an-ios-unified-api-project/
dsq_thread_id:
  - "3537844054"
categories:
  - Mobile
tags:
  - Xamarin
  - Xamarin iOS
  - Xamarin Linker
---
On a project my colleague <a href="https://twitter.com/halkar" target="_blank">@halkar</a>&nbsp;and I were working on, we came across a puzzling issue. We only discovered this issue after converting our project from the Classic MonoTouch API to the Unified API. On the iOS simulator, our application would run fine, but when we tried to run it against a real iOS device, we could get an error to the effect of “**Native linking failed, undefined Objective-C class …**”.

[<img class="alignnone wp-image-241 size-large" src="http://teamtam.net/wp-content/uploads/2015/02/NativeLinkingFailed-1024x396.png" alt="Xamarin - Native Linking Failed" width="1000" height="387" srcset="http://teamtam.net/wp-content/uploads/2015/02/NativeLinkingFailed-1024x396.png 1024w, http://teamtam.net/wp-content/uploads/2015/02/NativeLinkingFailed-300x116.png 300w, http://teamtam.net/wp-content/uploads/2015/02/NativeLinkingFailed-1000x386.png 1000w, http://teamtam.net/wp-content/uploads/2015/02/NativeLinkingFailed.png 1343w" sizes="(max-width: 1000px) 100vw, 1000px" />](http://teamtam.net/wp-content/uploads/2015/02/NativeLinkingFailed.png)

A quick search within our solution revealed no clues, and the subsequent Google search confirmed that **EAAccessoryManager** is indeed a native Objective-C class. So the class is clearly defined, but our compiler tells us otherwise?

As per the compiler error message, Google has multiple sources to suggest that the solution is to simply add a **[Protocol]** attribute to the offending C# class. However,&nbsp;here the code wasn’t in our application but in the Xamarin binding library that didn’t get linked to the corresponding Objective-C **EAAccessoryManager**.

Upon further investigation of the <a href="http://developer.xamarin.com/guides/ios/advanced_topics/registrar/" target="_blank">Xamarin.iOS Type Registrar</a> to explain why the application worked in the simulator but not a real device, it turns out that Xamarin uses dynamic registration for simulator builds but static registration for device builds. The reason for this is speed, in that a development machine would have the necessary power to scan classes at runtime, whereas a new binary is always required to deploy to a device. Going down this path, a suggested solution is to use the legacy registrar, but this just results&nbsp;in a different compiler error indicating&nbsp;it&nbsp;is not allowed in a Unified API project.

So where to from here?

**Enter the Xamarin Linker**

This led to us having a deeper look at the <a href="http://developer.xamarin.com/guides/ios/advanced_topics/linker/" target="_blank">Xamarin.iOS Linker</a>. In short, our project compiled when we selected “_Don’t link_” for the Linker behaviour option. The default option is “_Link SDK assemblies only_”, which will mean the Xamarin linker will attempt to exclude all the symbols from the SDK it thinks is unnecessary. Unfortunately, it appeared in our case the&nbsp;static registration analysis was over-zealous. It should be mentioned also that the use of the word “_Linker_” by Xamarin is slightly counter-intuitive <a href="http://forums.xamarin.com/discussion/12125/exact-behaviour-of-the-linker-in-xamarin-studio" target="_blank">as noted by Adam Kemp</a> and the behaviour is different to the conventional sense of the word. For my own understanding, I found it made more sense if it I substituted the word “_Optimize_” for “_Link_” – i.e. pick the “_Don’t optimize_” option!

[<img class="alignnone wp-image-252 size-large" src="http://teamtam.net/wp-content/uploads/2015/02/LinkerOptions-1024x483.png" alt="Xamarin - Linker Options" width="1000" height="472" srcset="http://teamtam.net/wp-content/uploads/2015/02/LinkerOptions-1024x483.png 1024w, http://teamtam.net/wp-content/uploads/2015/02/LinkerOptions-300x142.png 300w, http://teamtam.net/wp-content/uploads/2015/02/LinkerOptions-1000x472.png 1000w" sizes="(max-width: 1000px) 100vw, 1000px" />](http://teamtam.net/wp-content/uploads/2015/02/LinkerOptions.png)

**But it has to be at least … three times bigger than this!**

Please pardon the obscure Zoolander quote, but in practice&nbsp;you will&nbsp;need to consider the size of the generated&nbsp;IPA binary. It really depends on the exact nature of the application as to what the actual size difference would be, but roughly speaking it appears the &#8220;_Don&#8217;t link_&#8221; option can be 3-5 times larger than the default “_Link SDK assemblies only_”. App consumers these days are quite unforgiving, and so&nbsp;you really need to do everything in your power to provide a good first impression. This, of course, includes minimising the size of your app and therefore potential installation problems.

As such, the solution we settled for was to explicitly reference the **EAAccessoryManager** class in our code, allowing us to revert back to the“_Link SDK assemblies only_” option. This of course is merely a workaround, but it was the most elegant method we came up with to solve our problem. Hopefully Xamarin will address&nbsp;the issue with the Linker soon, but until then this workaround should do the trick.

<pre class="wp-code-highlight prettyprint">using ExternalAccessory;
...
var sharedAccessoryManager = EAAccessoryManager.SharedAccessoryManager;</pre>

Success! Time to go home.

**Addendum &#8211; Xamarin Support**

A big shout out to <a href="https://twitter.com/kumpera" target="_blank">@kumpera</a> and the Xamarin support team! We had a response within 24 hours after submitting a sample solution to replicate the problem. In our particular case, there were 2 issues that were at the root of the problem, neither of which was a fault with Xamarin. Firstly, a third party library we used did not contain ARM64 code, meaning&nbsp;linking failed when building for this architecture. Secondly, this library also had dependencies on frameworks such as the **EAAccessoryManager**. Anyway, instead of the workaround originally suggested, a better way to do achieve the same thing is to use the **LinkWith** attribute.

<pre class="wp-code-highlight prettyprint">[assembly: LinkWith (..., Frameworks = "ExternalAccessory")]</pre>

Once this attribute was added and we selected ARMV7 as the architecture to build for, we no longer experienced the issue originally described.

**Links**

  * <a title="http://developer.xamarin.com/guides/ios/advanced_topics/registrar/" href="http://developer.xamarin.com/guides/ios/advanced_topics/registrar/" target="_blank">http://developer.xamarin.com/guides/ios/advanced_topics/registrar/ </a>
  * <a title="http://developer.xamarin.com/guides/ios/advanced_topics/linker/" href="http://developer.xamarin.com/guides/ios/advanced_topics/linker/" target="_blank">http://developer.xamarin.com/guides/ios/advanced_topics/linker/</a>
  * <a title="http://forums.xamarin.com/discussion/12125/exact-behaviour-of-the-linker-in-xamarin-studio" href="http://forums.xamarin.com/discussion/12125/exact-behaviour-of-the-linker-in-xamarin-studio" target="_blank">http://forums.xamarin.com/discussion/12125/exact-behaviour-of-the-linker-in-xamarin-studio</a>