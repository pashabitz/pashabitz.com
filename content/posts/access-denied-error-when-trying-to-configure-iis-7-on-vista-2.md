---
title: '"Access Denied" Error When Trying to Configure IIS 7 on Vista'
date: Tue, 23 Oct 2007 14:58:50 +0000
draft: false
tags: ['Misc']
---

**Problem:** I get an "access denied" error message when trying to save some change done to a web application in the IIS manager on Vista.  
  
**Possible solution:**  
Windows Vista comes with a new version of IIS. One of the new things in IIS 7 is that it saves it's configuration **in the .net web.config** files (previous versions used the "IIS metabase", which is a bunch of files internal to the IIS configuration tool). This is an improvement because it simplifies deployment and versioning - all you need to do is edit an xml file, whereas the IIS metabase is configurable only using a special API.  
  
**Correction**(following Eyal's comment): actually the IIS metabase is an xml file that can be directly edited.  
  
So, why the "access denied"? If you're making an application-level (as opposed to machine-level) configuration, many times it's because your application's web.config file is not checked-out for edit in VS, and so it is read-only on disk. IIS manager cannot write to it and shows you the error.  
  
**More on IIS 7 at pashabitz.com:**  
[How to setup a custom HttpHandler in IIS 7](http://www.pashabitz.com/PermaLink,guid,985634fe-eb79-4c18-b03a-9a6be9b41cd1.aspx)  
[Developing ASP.NET apps under II7 on Vista.](http://www.pashabitz.com/PermaLink,guid,4311e751-7149-4094-8528-efaf4c00d99a.aspx)