---
id: 15
title: 'Xamarin.Forms &#8211; Test Drive'
date: 2014-10-08T12:00:49+00:00
author: TeamTam
layout: post
guid: http://teamtam.net/?p=15
permalink: /xamarin-forms-test-drive/
dsq_thread_id:
  - "3520358800"
categories:
  - Mobile
tags:
  - Reviews
  - Xamarin
  - Xamarin Forms
---
<p class="p1">
  Xamarin often quote figures in the order of 70% for code re-use across iOS, Android and Windows applications. The illustration below, borrowed from Xamarin.com, demonstrates this conceptually.
</p><figure id="attachment_96" style="width: 999px" class="wp-caption alignnone">

[<img class="size-full wp-image-96" src="http://teamtam.net/wp-content/uploads/2014/10/Xamarin-CodeSharing.png" alt="Xamarin Code Sharing Illustration" width="999" height="345" srcset="http://teamtam.net/wp-content/uploads/2014/10/Xamarin-CodeSharing.png 999w, http://teamtam.net/wp-content/uploads/2014/10/Xamarin-CodeSharing-300x103.png 300w" sizes="(max-width: 999px) 100vw, 999px" />](http://teamtam.net/wp-content/uploads/2014/10/Xamarin-CodeSharing.png)<figcaption class="wp-caption-text">Code Sharing with Xamarin</figcaption></figure> 

But what about Xamarin.Forms?&nbsp;Xamarin claim that now UI code can be shared across the 3 platforms, often 90% or an even higher percentage of code and be written once and re-used, depending on the exact nature of the application. Again borrowed Xamarin.com, the graphic below aims to illustrate this.<figure id="attachment_97" style="width: 1058px" class="wp-caption alignnone">

[<img class="size-full wp-image-97" src="http://teamtam.net/wp-content/uploads/2014/10/XamarinForms-CodeSharing.png" alt="Xamarin.Forms Code Sharing Illustration" width="1058" height="305" srcset="http://teamtam.net/wp-content/uploads/2014/10/XamarinForms-CodeSharing.png 1058w, http://teamtam.net/wp-content/uploads/2014/10/XamarinForms-CodeSharing-300x86.png 300w, http://teamtam.net/wp-content/uploads/2014/10/XamarinForms-CodeSharing-1024x295.png 1024w, http://teamtam.net/wp-content/uploads/2014/10/XamarinForms-CodeSharing-1000x288.png 1000w" sizes="(max-width: 1058px) 100vw, 1058px" />](http://teamtam.net/wp-content/uploads/2014/10/XamarinForms-CodeSharing.png)<figcaption class="wp-caption-text">Code Sharing with Xamarin.Forms</figcaption></figure> 

So how valid&nbsp;are Xamarin&#8217;s claims, or is this just marketing hype? Are the quoted percentages too good to be true? Are the figures reflective of the lines of code, time and effort spent or some other random metric? What does this look like in practical terms? Let&#8217;s explore a little.

**Case Study: Simple Timesheet App**

For the purpose of this exercise, we will build a simple timesheet app inspired by a real project at Readify. We will define&nbsp;two user stories with acceptance criteria as per the below.

1. View Timesheet
  
As a user, I want to be able to view a list of my outstanding timesheets, so I can see which timesheets I haven’t submitted yet
  
&#8211; Date, Customer, Project&nbsp;read-only fields
  
&#8211; Loading indicator displayed while waiting for a response

2. Submit Timesheet
  
As a user, I want to be able to submit timesheets, to allow the business to invoice accurately
  
&#8211; Date, Customer, Project&nbsp;read-only fields
  
&#8211; Hours (.25 hour increments – 0 to 24) selection entry
  
&#8211; Comment text entry
  
&#8211; Sick Leave (yes/no) selection entry
  
&#8211; Submit button
  
&#8211; Loading indicator displayed while waiting for a response
  
&#8211; If successful, the user will be returned to the timesheet list page
  
&#8211; If unsuccessful, a friendly error will be displayed to the user

<p class="p1">
  <strong>Under The Bonnet</strong>
</p>

<p class="p1">
  Before you can use Xamarin.Forms, you will need at least an Indie license. Alternatively, you can obtain a trial Business license.
</p>

<p class="p1">
  So, let’s set up the building blocks for our test drive. Our solution should contain 4 projects: one for iOS, Android and WinPhone, and of course a common project that contains all of our shared code where all&nbsp;of the action will be located unless noted otherwise. The common project can either be a Portable Class Library (PCL) or a Shared Project, but for this exercise we will implement it as a PCL.
</p>

<p class="p1">
  One final note before we proceed any further &#8211; this blog post does not aim to be a tutorial so implementation details will not always be described at length, but they will be mentioned if they are a little out of the ordinary. However, <a title="https://github.com/teamtam/xamarin-forms-timesheet" href="https://github.com/teamtam/xamarin-forms-timesheet" target="_blank">all code is available on GitHub</a> so you can follow along or examine anything in detail should you wish to, with commits corresponding roughly to the flow of this blog.
</p>

<p class="p1">
  To&nbsp;model our data, we create a TimesheetEntry.cs class and put it under &#8216;Models&#8217;.
</p><figure id="attachment_49" style="width: 300px" class="wp-caption alignnone">

[<img class="wp-image-49 size-medium" src="http://teamtam.net/wp-content/uploads/2014/09/TimesheetEntry-300x194.png" alt="Timesheet Entry" width="300" height="194" srcset="http://teamtam.net/wp-content/uploads/2014/09/TimesheetEntry-300x194.png 300w, http://teamtam.net/wp-content/uploads/2014/09/TimesheetEntry-1000x649.png 1000w, http://teamtam.net/wp-content/uploads/2014/09/TimesheetEntry.png 1010w" sizes="(max-width: 300px) 100vw, 300px" />](http://teamtam.net/wp-content/uploads/2014/09/TimesheetEntry.png)<figcaption class="wp-caption-text">TimesheetEntry.cs class</figcaption></figure> 

<p class="p1">
  We create a service to handle the listing and submission of timesheet entries and put it under &#8216;Services&#8217; like the below. Note that our implementation does not do anything like persist data &#8211; for the purpose of this exercise, we are aiming to do just enough to examine Xamarin.Forms in a meaningful way. So, our listing returns the same data every time and our submission won’t really save, but we&nbsp;make each method sleep for 2 seconds to simulate a call to an external data source over the wire.
</p><figure id="attachment_50" style="width: 300px" class="wp-caption alignnone">

[<img class="wp-image-50 size-medium" src="http://teamtam.net/wp-content/uploads/2014/09/TimesheetService-300x195.png" alt="Timesheet Service" width="300" height="195" srcset="http://teamtam.net/wp-content/uploads/2014/09/TimesheetService-300x195.png 300w, http://teamtam.net/wp-content/uploads/2014/09/TimesheetService-1000x652.png 1000w, http://teamtam.net/wp-content/uploads/2014/09/TimesheetService.png 1007w" sizes="(max-width: 300px) 100vw, 300px" />](http://teamtam.net/wp-content/uploads/2014/09/TimesheetService.png)<figcaption class="wp-caption-text">TimesheetService.cs class</figcaption></figure> 

<p class="p1">
  <strong>Ready, Set, Go!</strong>
</p>

<p class="p1">
  We can now concentrate on our UI code to see if Xamarin.Forms can deliver on what it promises. With Xamarin.Forms, you can either work in XAML or code, and here we will work with the latter. Both arguably have their advantages, such as a better separation of concerns versus the current availability of intellisense in Visual Studio respectively.
</p>

<p class="p1">
  Firstly, we create the&nbsp;outstanding timesheet list page using a ListView control. ListView works well without much effort, but because our user story has identified 3 fields to display, we need to create a custom ViewCell for it as the standard implementation of&nbsp;ListView only allows for 2 lines of text. Our custom ViewCell will contain a StackLayout of our 3 fields.
</p><figure id="attachment_59" style="width: 1053px" class="wp-caption alignnone">

[<img class="wp-image-59 size-full" src="http://teamtam.net/wp-content/uploads/2014/09/TimesheetListPage1.png" alt="Timesheet List Page" width="1053" height="467" srcset="http://teamtam.net/wp-content/uploads/2014/09/TimesheetListPage1.png 1053w, http://teamtam.net/wp-content/uploads/2014/09/TimesheetListPage1-300x133.png 300w, http://teamtam.net/wp-content/uploads/2014/09/TimesheetListPage1-1024x454.png 1024w, http://teamtam.net/wp-content/uploads/2014/09/TimesheetListPage1-1000x443.png 1000w" sizes="(max-width: 1053px) 100vw, 1053px" />](http://teamtam.net/wp-content/uploads/2014/09/TimesheetListPage1.png)<figcaption class="wp-caption-text">Timesheet List Page, Displayed on Windows Phone</figcaption></figure> 

The timesheet list page was easy enough to create with the only hurdle being each ViewCell requiring a height value. Otherwise, Xamarin.Forms has been adept at calculating screen real estate to this point, although there is some variation between how the list looks on iOS and Windows Phone. In our example, a ViewCell height of 80 works better on iOS and 100 on Windows. The official Xamarin.Forms documentation does acknowledge the issue, so perhaps Xamarin will address it in the near future.<figure id="attachment_46" style="width: 709px" class="wp-caption alignnone">

[<img class="wp-image-46 size-full" src="http://teamtam.net/wp-content/uploads/2014/09/ListViewCellHeight.png" alt="ListView Cell Height" width="709" height="95" srcset="http://teamtam.net/wp-content/uploads/2014/09/ListViewCellHeight.png 709w, http://teamtam.net/wp-content/uploads/2014/09/ListViewCellHeight-300x40.png 300w" sizes="(max-width: 709px) 100vw, 709px" />](http://teamtam.net/wp-content/uploads/2014/09/ListViewCellHeight.png)<figcaption class="wp-caption-text">Xamarin&#8217;s Fine Print on ViewCell</figcaption></figure> 

For the timesheet entry&nbsp;page, we will use a vertical StackLayout control for the overarching structure of the page, with a nested horizontal StackLayout for each label and field combination.<figure id="attachment_58" style="width: 1047px" class="wp-caption alignnone">

[<img class="size-full wp-image-58" src="http://teamtam.net/wp-content/uploads/2014/09/TimesheetEntryPage1.png" alt="Timesheet Entry Page" width="1047" height="458" srcset="http://teamtam.net/wp-content/uploads/2014/09/TimesheetEntryPage1.png 1047w, http://teamtam.net/wp-content/uploads/2014/09/TimesheetEntryPage1-300x131.png 300w, http://teamtam.net/wp-content/uploads/2014/09/TimesheetEntryPage1-1024x447.png 1024w, http://teamtam.net/wp-content/uploads/2014/09/TimesheetEntryPage1-1000x437.png 1000w" sizes="(max-width: 1047px) 100vw, 1047px" />](http://teamtam.net/wp-content/uploads/2014/09/TimesheetEntryPage1.png)<figcaption class="wp-caption-text">Timesheet Entry Page, Displayed on&nbsp;Windows Phone</figcaption></figure> 

From my experience, the basic layout and functionality for this page was achieved without significant loss of momentum. However, in&nbsp;working with the&nbsp;controls, I discovered a number of quirks with them&nbsp;and so&nbsp;I did&nbsp;get bogged down occasionally. In the opinion of this author, some of these issues can arguably be expected, while others not so much. As alluded to earlier, spacing, padding and alignment exhibited some variance between devices, which can perhaps be excused especially by&nbsp;developers coming from a web background where catering for multiple browsers is the norm. However, LayoutOptions that can be used with the StackLayout control behave inconsistently and unexpectedly for a singular device, let alone when compared between iOS, Android and Windows platforms. For example, it was by trial-and-error to determine whether to use End, EndAndExpand, Fill or&nbsp;FillAndExpand for the LayoutOptions of the &#8216;Hours&#8217;, &#8216;Comments&#8217; and &#8216;Sick Leave&#8217;&nbsp;input fields. In addition, the&nbsp;width of a label cannot be guaranteed with the developer only able to request a width, leading again to layout differences&nbsp;across devices. In hindsight, perhaps using GridLayout or rethinking the layout altogether would have been a better choice for this page instead of nested StackLayouts. When I get a chance to investigate the effect of GridLayout, I will update this post accordingly&nbsp;should there be anything pertinent to report.

**Not So Fast &#8230;**

Things get even more interesting once the ActivityIndicator control gets introduced. While the documentation for the Xamarin.Forms control gallery is generally quite good, it is very sparse&nbsp;for the ActivityIndicator and does not provide any context or examples of how to use it.<figure id="attachment_45" style="width: 300px" class="wp-caption alignright">

[<img class="wp-image-45 size-medium" src="http://teamtam.net/wp-content/uploads/2014/09/ActivityIndicator-300x207.png" alt="Activity Indicator Documentation" width="300" height="207" srcset="http://teamtam.net/wp-content/uploads/2014/09/ActivityIndicator-300x207.png 300w, http://teamtam.net/wp-content/uploads/2014/09/ActivityIndicator-1024x709.png 1024w, http://teamtam.net/wp-content/uploads/2014/09/ActivityIndicator-1000x692.png 1000w, http://teamtam.net/wp-content/uploads/2014/09/ActivityIndicator.png 1149w" sizes="(max-width: 300px) 100vw, 300px" />](http://teamtam.net/wp-content/uploads/2014/09/ActivityIndicator.png)<figcaption class="wp-caption-text">Xamarin&#8217;s ActivityIndicator Documentation &#8230;</figcaption></figure> 

<p class="p1">
  So where to from here? For the live project that actually preceded this exercise, we inferred the path forward with the ActivityIndicator by reading between the lines of other Xamarin.Forms documentation, with prior experience of Windows Phone or WPF on the team making this process faster. The answer we eventually arrived at: MVVM. When doing this for the first time in the real world&nbsp;we appeared stalled on the ActivityIndicator&nbsp;for a while, but providing the user with visibility when something is loading remained an important feature for our product owner, and rightfully so.
</p>

<p class="p1">
  To cater for MVVM, we add View Models (VM) to sit between the Views (V) and Models (M) that we already have. In our solution, we also rename the &#8216;Pages&#8217; folder and namespace to &#8216;Views&#8217; to match MVVM nomenclature. Note that there is some overlap of verbiage which can cause confusion, with&nbsp;Views referring either to Xamarin.Forms controls or the MVVM sense of the word. In addition, we use a Fody add-in (available via NuGet) to allow MVVM data annotations. However, another quirk discovered with Xamarin.Forms is that data binding of the Picker control is not yet supported.
</p>

<p class="p1">
  Now that our solution has&nbsp;the MVVM building blocks in place, the ActivityIndicator is&nbsp;bound easily to the ‘IsLoading’ property in our ViewModels&nbsp;so it displays at the appropriate times. In our particular implementation, there is a ‘BaseViewModel’ with the ‘IsLoading’ property that other view models inherit from.
</p>

<p class="p1">
  However, getting the ActivityIndicator to display, and getting it to display where you want it to is an entirely different story. Placing the loading indicator at the centre is&nbsp;a common design paradigm in apps, and so the first approach attempted was to overlay the ActivityIndicator using RelativeLayout with this in mind, and the second was to likewise try AbsoluteLayout &#8211; both yielded similar results. Despite there being no changes apart from overlaying the ActivityIndicator with either relative or absolute layout, the scrollable area on the listing page now gets cut off when the list is pulled down as if&nbsp;to refresh data on a typical app. Meanwhile, the alignment and padding on the entry page goes askew no matter which layout is&nbsp;used. In the series of screenshots below as displayed on the&nbsp;iPhone emulator, the affected areas described have been highlighted in purple.
</p><figure id="attachment_94" style="width: 1024px" class="wp-caption alignnone">

[<img class="wp-image-94 size-full" src="http://teamtam.net/wp-content/uploads/2014/10/Listing-ActivityIndicator.png" alt="Listing-ActivityIndicator" width="1024" height="712" srcset="http://teamtam.net/wp-content/uploads/2014/10/Listing-ActivityIndicator.png 1024w, http://teamtam.net/wp-content/uploads/2014/10/Listing-ActivityIndicator-300x208.png 300w, http://teamtam.net/wp-content/uploads/2014/10/Listing-ActivityIndicator-1000x695.png 1000w" sizes="(max-width: 1024px) 100vw, 1024px" />](http://teamtam.net/wp-content/uploads/2014/10/Listing-ActivityIndicator.png)<figcaption class="wp-caption-text">Effect of the Activity Indicator on the Listing page</figcaption></figure> <figure id="attachment_93" style="width: 1024px" class="wp-caption alignnone">[<img class="wp-image-93 size-full" src="http://teamtam.net/wp-content/uploads/2014/10/Entry-ActivityIndicator.png" alt="Entry-ActivityIndicator" width="1024" height="712" srcset="http://teamtam.net/wp-content/uploads/2014/10/Entry-ActivityIndicator.png 1024w, http://teamtam.net/wp-content/uploads/2014/10/Entry-ActivityIndicator-300x208.png 300w, http://teamtam.net/wp-content/uploads/2014/10/Entry-ActivityIndicator-1000x695.png 1000w" sizes="(max-width: 1024px) 100vw, 1024px" />](http://teamtam.net/wp-content/uploads/2014/10/Entry-ActivityIndicator.png)<figcaption class="wp-caption-text">Effect of the Activity Indicator on the Entry page</figcaption></figure> 

<p class="p1">
  To be fair, Xamarin do realise their layout controls need more attention and have been transparent about this.
</p><figure id="attachment_47" style="width: 722px" class="wp-caption alignnone">

[<img class="size-full wp-image-47" src="http://teamtam.net/wp-content/uploads/2014/09/ListViewLayout.png" alt="List View Layout" width="722" height="121" srcset="http://teamtam.net/wp-content/uploads/2014/09/ListViewLayout.png 722w, http://teamtam.net/wp-content/uploads/2014/09/ListViewLayout-300x50.png 300w" sizes="(max-width: 722px) 100vw, 722px" />](http://teamtam.net/wp-content/uploads/2014/09/ListViewLayout.png)<figcaption class="wp-caption-text">Xamarin&#8217;s Fine Print on ListView</figcaption></figure> 

<p class="p1">
  <strong>Final Thoughts</strong>
</p>

<p class="p1">
  In terms of sharing UI logic, Xamarin.Forms does theoretically deliver. In our simple example, we created&nbsp;zero platform-specific UI code and so any figures Xamarin quote in the order of 90%-100% I would technically find believable.
</p>

<p class="p1">
  However, whether such figures are meaningful in the real world is another story. Layouts are problematic to work with and is somewhat of a time-consuming, trial-and-error process, especially once they get nested. Chances are, significant time will be spent getting your applications to look consistent across devices too, but instead of developing platform specific UI logic, effort will be required to find solutions or workarounds for the controls provided by Xamarin.Forms. Other controls,&nbsp;such as the Picker or the ActivityIndicator, have yet to be implemented or documented properly respectively.
</p>

<p class="p1">
  Overall, Xamarin.Forms undoubted has enormous potential, but I would suggest that it is not quite ready for production&nbsp;yet except perhaps for apps with reasonably simple layouts, prototypes, or if your product owner is flexible and does not require the app to be pixel-perfect. It will be very interesting indeed to see how it matures in the near future as Xamarin&nbsp;appear to be investing into this offering, and I have seen evidence of progress in the brief time I have worked with the technology. As such, despite present shortcomings, I am optimistic for the future of Xamarin.Forms&nbsp;and will be keeping a close eye on it.
</p>

<p class="p1">
  <strong>Links</strong>
</p>

  * <a title="https://github.com/teamtam/xamarin-forms-timesheet" href="https://github.com/teamtam/xamarin-forms-timesheet" target="_blank">https://github.com/teamtam/xamarin-forms-timesheet</a>
  * <a title="http://developer.xamarin.com/guides/cross-platform/xamarin-forms/controls/" href="http://developer.xamarin.com/guides/cross-platform/xamarin-forms/controls/" target="_blank">http://developer.xamarin.com/guides/cross-platform/xamarin-forms/controls/</a>