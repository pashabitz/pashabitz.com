---
title: 'Develop ASP.NET Apps under IIS7 on Vista'
date: Thu, 02 Aug 2007 06:43:48 +0000
draft: false
tags: ['Misc']
---

I couldn't find this all in one place, so, here's how you enable developing and debugging an asp.net app under IIS7 on Vista:  
  
1 Install windows features in "turn windows features on or off". You need these things (which are not installed by default):  
Under "web management tools":  
a. IIS metabase and IIS 6 configuration compatibility  
b. IIS management console  
Under "Application Development Features":  
a. ASP.NET  
Under "Common Http Features" - check everything.  
  
![](/wp-content/uploads/2015/10/windows_features2.gif)  
  
2 Open the web application project in VS, go to proprties, under "Web":  
 2.1 Select "Use IIS Web Server".  
 2.2 Click "Create Virtual Dir" (this actualy creates an application, not virtual directory on IIS 7).  
3 Open IIS manager:  
 3.1 Make sure the default web site is started.  
 3.2 Select your application:  
  3.2.1 Click "basic settings...", choose under "Application Pool" the option "classic .Net AppPool"  
  3.2.2 Under "authentication" enable "anonymous authentication"  
  3.2.3 If you use windows/forms/passport authentication in your asp.net app. - you need additional configuration.  
4 In VS set your startup page (right click on desired page in solution explorer)  
5\. Run  
  
Note: debugging still shouldn't work at this point, only "start without debug".  
Note 2: many places say that you must run VS as administrator (not the default way under Vista), but for me it seems to work when running not as admin as well.  
  
To enable debugging:  
1\. Install this [hotfix](http://support.microsoft.com/default.aspx?scid=kb;EN-US;937523). Download [here](http://connect.microsoft.com/VisualStudio/Downloads/DownloadDetails.aspx?DownloadID=7250).  
Now debugging should work also.  
  
  
**Related on pashabitz.com:** [SSL in ASP.NET - part I](http://www.pashabitz.com/PermaLink,guid,301fc298-309e-4391-a254-72c9c8c523d2.aspx) | [SSL in ASP.NET - part II](http://www.pashabitz.com/PermaLink,guid,bb721a49-99ec-4b92-b5ba-b001a80d5dde.aspx).