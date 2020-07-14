---
title: 'MySQL dates quiz - Answer'
date: Wed, 03 Oct 2007 20:50:30 +0000
draft: false
tags: ['Misc']
---

That was fast.  
[Oren](http://www.lnbogen.com) wins a beer correctly answering [the quiz](http://www.pashabitz.com/PermaLink,guid,5bffe1b0-3de2-4fbd-9ae4-8bc999b1e9dd.aspx).  
Not exactly what I had in mind, but the rules didn't say anything about googling it.  
  
**Solution:**  
MySQL thinks that in my time zone (Jerusalem) daylight savings time starts on March 30th every year (which is [not exactly true](http://en.wikipedia.org/wiki/Daylight_saving_time_around_the_world#Israel)) and on that day the clock goes from 01:59:59AM straight to 03:00:00AM. So all the datetime values between 30/3 02:00:00 and 30/3 02:59:59 are considered invalid.  
  
Also funny how another person who ran into this [thought it was quiz material](http://benjisimon.blogspot.com/2007/08/sql-brain-teaser-why-incorrect-datetime.html).