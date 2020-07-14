---
title: 'Don''t Use HttpUtility.UrlEncode for Anything But Wiping your Butt'
date: Tue, 22 Jan 2008 10:07:19 +0000
draft: false
tags: ['Misc']
---

Like written so many times before, HttpUtility.UrlEncode is extremely outdated and buggy.  
It doesn't encode the single quote ('), there's [an open bug marked "Closed (Won't Fix)"](http://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=252514). So don't wait for a patch.  
It doesn't encode the plus sign (+) either.  
  
So please use the [AntiXss library](http://www.microsoft.com/downloads/details.aspx?FamilyId=EFB9C819-53FF-4F82-BFAF-E11625130C25&displaylang=en) whenever it's remotely important to get it right.  
Needless to say, same goes for the other encoding methods of HttpUtility, like HtmlEncode.