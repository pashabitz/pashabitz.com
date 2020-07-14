---
title: 'AJAX-enabled Web Control - Followup #1'
date: Sun, 11 Sep 2005 17:42:47 +0000
draft: false
tags: ['Misc']
---

Okay boys and girls, read my post about the little kinky control I'm doing [here](http://www.pashabitz.com/PermaLink,guid,5129a036-2a84-49d5-9693-49a3ba88b585.aspx) first.

Today I decided to have a go at fixing problem #2, which looked like it could lead to solving #1 and #3. One may say it's a _problem-solving problem-solution_. Right. So.

As you remember, I didn't like having a regular ASP.NET page acting as my HTTP handler. That's because it's not (1)deployable, (2)general, probably not (3)too-well-performing, and well (4) smells.

So I thought I'll implement it as a proper ASP.NET HTTP Handler.

Here's how you do it:

1.  Write a class that implements System.Web.IHttpHandler.  
    Here I already have a feeling how I'm going to solve the deployment issue: I put my IHttpHandler implementation in the same assembly the WebControl lives in. Now they're deployed together.
2.  All you really care about here is to implement the method ProcessRequest. You are given an HttpContext context parameter, and that parameter can give you everything the Page gives you, like Request, Response and so on. Notice that the Page class itself implements IHttpHandler. Basically what you're expected to do here is look at the HTTP request you got and output some HTTP response. Fun.
3.  My initial solution did that in the Page\_Load of the handler Page. So all I had to do is move all my code to the ProcessRequest of my IHttpHandler implementation and add **context.** to all the Response, Request and Session uses I had inside the code. 
4.  If you need to access the Session, like me, mark your IHttpHandler with System.Web.SessionState.IRequiresSessionState marker interface.
5.  Register the handler with the web.config of the application:  
    I added <add verb="\*" path="\*.mspx" type="HandlerClassName, AssemblyName" /> in the <httpHandlers> section of <system.web>.  
    The **verb** is what HTTP verbs you want your handler to catch (POST, GET, PUT...).  
    **path** - requests for what resources you want the handler to catch (I chose to catch all files with an .mspx extension, I'll probably change that to something more specific). Notice you don't really need a specific file like "TheBitz.mspx" to exist on your server. You won't get a 404.  
    **type** - that's the pointer to your IHttpHandler implementation.
6.  Register the handler with IIS. This one is just a little bit tricky:  
    Go to the IIS MMC -> right-click the wanted web site -> Properties -> Home Directory -> Configuration... -> Add. Now point the wanted extension (in my example ".mspx") to the ASP.NET dll. (Just copy it's path from the .aspx line)

Now I replaced the URL in the client-side XMLHTTP request to some URL on the web site with an .mspx extension and the request went to the newly created custom HTTP handler. Pretty easy.

But here's an annoying issue I'm having:

I'm calling an old COM component (VB6) from that handler. Remember that .NET and COM threading models don't match (MTA vs. STA). Now if you wanted to call COM components from an ASP.NET page you marked it with an ASPCOMPAT=true attribute in the @Page directive. That forced the page to be handled on an STA thread. But I have no clue as to how to force my custom hadler to run in an STA thread. I tried:

1.  Marking the ProcessRequest method with and STAThreadAttribute.
2.  Calling Thread.CurrentThread.ApartmentState = ApartmentState.STA.

Both don't work. The damn model stays MTA. And that's logical, why should I expect to change the threading model after the Thread started? That was just plain idiocy of me I guess.

So I'm pretty unhappy right now.

If you have any tips, do write. Thanks.