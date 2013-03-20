---
layout: post
title: "Objective-C Blocks vs NSNotificationCenter for async completion"
date: 2012-12-31 21:43
comments: true
categories: [Objective-C, Software Development]
---
I'm working on a project that was created pre iOS 4.0, and is now only supporting iOS 4 and later. This means I am able to finally use [Blocks](http://developer.apple.com/library/ios/#documentation/cocoa/Conceptual/Blocks/Articles/00_Introduction.html). So what was being used to inform an object when a called async method passed or failed? [NSNotificationCenter](https://developer.apple.com/library/mac/#documentation/Cocoa/Reference/Foundation/Classes/NSNotificationCenter_Class/Reference/Reference.html). This was the original code and it worked pretty well:

{% gist 4422401 %}

It allowed the method getSubscriptionFromServer to inform the caller in the Subscription class to know when getSubscriptionFromServer was complete, or failed. This might not have been the best way to handle this, but it doesn't matter now.

Luckly I was able to make some changes, like dropping iOS 3.0 support and included the use of Blocks. This alowed me to change the above code to something like the following:

{% gist 4422453 %}

This helped clean up the code and encapsulate a lot of what was going on. The person using the Datacontroller class no longer needed to know about the Notifications. The method signature will now tell them all they need to know inorder to call getSubscriptionFromServer.

While Blocks is nothing new, I was happy to be able to finally use them to help clean up the codebase.
