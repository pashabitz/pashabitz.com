---
title: 'Twitter API .Net Libraries Roundup'
date: Mon, 07 Feb 2011 20:03:05 +0000
draft: false
tags: ['Code', 'Misc']
---

Out of the few [Twitter API libraries](http://dev.twitter.com/pages/libraries#dotnet) for .net out there, the ones that seem most complete are: [Twitterizer](http://www.twitterizer.net/) and [TweetSharp](http://tweetsharp.codeplex.com/).  
I've been developing with both in the past couple of weeks and both are generally stable, complete and provide a clean one-per-one wrapper for the Twitter API methods. Which is great news (tm).  
TweetSharp currently does not support the Streaming API, which is a major drawback and the reason why I am sticking with Twitterizer for now.  
  
I recommend using the [latest 2.3.1 source code](http://www.twitterizer.net/files/Twitterizer2.3.1-source.zip) of Twitterizer.  
  
Small but important note: Twitterizer's team is not currently officially supporting the Streaming API, so to use it (in assembly Twitterizer2.Streaming) - **comment out** the following line at the end of _TwitterStream.StartStream_:  
  
request.Abort();  
  
Otherwise - it will not work.