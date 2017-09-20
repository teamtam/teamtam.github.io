---
id: 11
title: 'Java To C# &#8211; There &#038; Back Again'
date: 2016-01-06T22:00:21+00:00
author: TeamTam
layout: post
guid: http://teamtam.net/?p=11
permalink: /java-to-csharp/
dsq_thread_id:
  - "4469594618"
categories:
  - Languages
tags:
  - 'C#'
  - Java
  - Musings
---
Many years ago, I used to be a Java developer. I liked it, and even held elitist attitudes against .NET when it first came on the scene in response to Java. At the time I thought, &#8220;there was Microsoft years behind yet again, not innovating but merely copying the market leader!&#8221; Later, the organisation I was working for made &#8220;strategic&#8221; technology changes to go with the whims of new management (without consulting the team of course). Before I knew it, I was developing in C# on the .NET stack. Kicking and screaming all the way, but mind you, _professionally_ kicking and screaming all the way.

Anyway during the recent Christmas break, I decided to get my Java back on and have a play with native Android on a pet project. Yes I do realise that Android has its own implementation of Java, only adopting its syntax and semantics. So for the purpose of this post, I will focus on the language and not the platform or Android specifics. Within several hours however, I already had a list of Java features that were frustrating me or I was yearning for! Without further ado &#8230;

**1. Decimal Numbers**
  
I was shocked that in the 10 years or so I have been away from the language on a day-to-day level, Java _still_ does not have a more elegant and concise way to deal with decimal numbers. They are still second class citizens &#8211; I don&#8217;t want to have to deal with a boxed object every time I work with currency! The decimal type in .NET is much more convenient to work with. Surely Java could provide arithmetic overloaded operands for BigDecimal at the very least?!?

**2. Properties and Object Initializers**
  
While properties and object initializers are really just &#8220;syntactic sugar&#8221;, I do appreciate how they make code more concise. More specifically with properties, there is no longer the need to have explicit setter/getter methods to access private fields, while object initializers alleviate the need to have multiple constructors catering for every parameter permutation. Admittedly, if I hadn&#8217;t gotten used to this syntax in C#, I probably wouldn&#8217;t have noticed their absence in Java. But I have, so I did.

**3. Implicit Typing**
  
I surprised myself when I realised I missed being able to declare &#8220;var&#8221; instead of the explicit type. When the feature was introduced to C#, I was skeptical before slowly embracing it. I know this is a contentious issue and there is still a debate on it &#8230; but these days I know how I prefer to code!

**4. Asynchronous-ness**
  
Since .NET 4.5 onwards until facing life in Java without them, I took for granted just how awesome the async/await operators are. It&#8217;s quite impressive when you look in depth at the work Microsoft have done to make asynchronous programming so easy &#8211; the compiler generates a state machine for each async declaration! Of my wish-list for Java, this is admittedly the most ambitious, but it would certainly be nice.

I never thought I would say this back when I was primarily developing in Java, but I really like C#! It is no longer a necessary evil forced upon by circumstance, but now my language of choice. As it stands today, C# is more concise, elegant and progressive than Java. Furthermore if you look at the history of release cycles, Microsoft is evolving and iterating the language faster than Oracle. As such I have every confidence that C# will continue to provide a great developer experience for the foreseeable future. However, if there is one thing I have learnt in my time in the industry is to not be dogmatic about technology, as demonstrated by the swinging fortunes of Java and C#. But for now, C# FTW! 🙂