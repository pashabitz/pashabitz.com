---
title: 'Writing an AJAX-enabled Web Control'
date: Sat, 10 Sep 2005 14:29:40 +0000
draft: false
tags: ['Misc']
---

Troubling Thought #1 - "AJAX-enabled", is that a word??

On we go.

At work, we have us a nice Web Control. Currently I'm adding to it some functionality that will get data from the server without posting-back the whole page.

So I started with implementing this stuff in the most straightforward and naive way possible, because I wanted to see it work. Here's how:

1\. I added some custom javascript to the control's behavior file. That javascript catches some client-side events, creates and XMLHTTP object, builds a custom request in XML format, sends it to the server, gets an XML response back, decodes it and displays the data to the user.

2\. On the server I wrote me a simple asp.net page(that's ver. 1.1).

In that page's Page\_Load I call Request.BinaryRead to get the request XML, then I decode the request, get the needed data, build an XML out of it and then Response.Write it back to the client.

And the amazing part is - it works.

So now I'm just left not too happy with the implementation. Here are the issues I see:

1\. I don't like to see that custom handling of XML encoding/decoding and XMLHTTP-related code in the javascript. Ideally, I want some framework that will handle that for me. What I want is to call a client-side function and have some server-side method get (magically) called for me.

This is exactly what the cool [ajax.net](http://ajax.schwarz-interactive.de/csharpsample/default.aspx) framework does, but it does so for Pages. Your server-side method has to reside in some specific Page. And me, I have a web control, not a specific page.

2\. Using a standard asp.net Page as an HTTP handler doesn't smell right. And probably performance-wise it's not best either. I probably should implement some IHttpHandler or something (at least that's what I think they're called). I read once that's as easy as getting porn on the internet. But I never actually did it. (The former, that is)

3\. On the same note, how am I to deploy the web control now? Telling the users to put a specially designated .aspx file at a fixed location in their web app isn't all that classy, is it?

That's it for the real stinking stuff I guess.

I'd appreciate your take on the three issues, and some additional suggestions while you're at it, if you have them.

Thanks.