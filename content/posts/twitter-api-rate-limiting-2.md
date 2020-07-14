---
title: 'Twitter API Rate Limiting'
date: Sat, 22 Jan 2011 10:59:15 +0000
draft: false
tags: ['Code', 'Misc']
---

Reading [http://dev.twitter.com/pages/rate-limiting](http://dev.twitter.com/pages/rate-limiting), here are the major points:  

REST API
--------

*   Anonymous calls (things like _users/show_ - get a user's info) are limited to 150/hour, that's 3,600/day. This limit is IP-based.  
    
*   Authenticated calls (like a user's home timeline) - 300/hour. Limit based on your app key.  
    

Those rates are useful for a single-user app (something like TweetDeck), not useful for something like an app that crawls Twitter.  
You can ask to be white-listed using this form: [http://twitter.com/help/request\_whitelisting](http://twitter.com/help/request_whitelisting).  
If approved - you get 20,000 requests / hour. That's more useful.  
  

Search API
----------

*   The rates are limited but to a higher rate than the REST API. The exact number is not disclosed.  
    
*   Important to include a _User Agent_ parameter, otherwise you will get a lower limit.  
    

  

Streaming API
-------------

*   This is what you really need for high-volume Twitter polling. Gives you a sampling of all tweets based on optional filter you pass (e.g. user, keyword, location).  
    
*   Have three access levels, based on what Twitter decides to give you: Spritzer (about 1% of everything), Gardenhose (10%) and Firehose (everything).