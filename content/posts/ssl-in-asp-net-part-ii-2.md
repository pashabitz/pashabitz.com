---
title: 'SSL in ASP.NET - Part II'
date: Tue, 13 Mar 2007 11:21:00 +0000
draft: false
tags: ['Misc']
---

This is the second part in an article series about setting up SSL in an ASP.NET application.

You can read the [first part here](http://www.pashabitz.com/PermaLink,guid,301fc298-309e-4391-a254-72c9c8c523d2.aspx "first part here"). Go ahead, read it now.

Okay.

Now, that we've created an SSL certificate for testing and development purposes, we are ready to make the required configuration in IIS.

Setting Up IIS to Work with SSL

First thing we have to do is configure the web site to use the certificate we created:

1.  From the IIS MMC snap-in, select your web site, right-click "properties" and under "directory security" click "Server Certificate...".
2.  Click "Assign an existing certificate". You should be able to see the self-signed certificate you created. Select it and finish the wizard.

At this state IIS is able to respond to SSL HTTP requests with this certificate.

To test that everything is okay, try to navigate to an existing URL in your application, with **https** in the beginning of the URL.

**Forcing SSL for an application**

If your application requires SSL encryption for all traffic you want to force the application to only handle SSL requests.

You can do this on the application level (so that other applications on the same web site in IIS will not require SSL) or on the web site level.

Here's how:

1.  In IIS MMC, right click the application virtual directory or the web site.
2.  Select "Directory Security" tab and click "Edit...".
3.  Check "Require secure channel" and "Require 128-bit encryption".

Now, any request to a URL that starts with **http** and not **https** - will receive a 403.4 error from the web server.

**Supporting Debugging in Visual Studio**

Now that you've setup the web server on your machine to require SSL traffic, you need to update the application URL in Visual Studio in order to be able to run the application from Visual Studio:

1.  Right click the project in Solution Explorer and select "Properties".
2.  Under the "Web" tab change the value of "Project Url" to start with **https**.

**Auto-Redirecting non-SSL Traffic**

Perhaps some of your users will try to navigate to your application via a non SSL URL (most users assume a web page URL starts with http).

You can silently redirect your users to the correct SSL address using the following technique:

Users trying to reach a non-SSL URL will be automatically redirected by IIS to the standard 403.4 error page. First step is to change that page to your own page:

1.  In your project, create a new web form called "NonSslRedirect.aspx".
2.  In IIS MMC, right click your application virtual directory, click the "Custom Errors" tab and select the 403;4 error.
3.  Click "Edit Properties...", select "URL" in "Message Type" and type the address of the page you added in step 1 (for example: **/MyApp/NonSslRedirect.aspx**).

Try again to navigate to a URL starting with http. You should be redirected by IIS to NonSslRedirect.aspx.

Now, in the code-behind file of NonSslRedirect.aspx, add code similar to the following to automatically redirect the users to the matching SSL URL:

protected void Page\_Load(object sender, EventArgs e)

{

string originalUrl = Request.Url.ToString().Split(new char\[1\] { ';' })\[1\];

string urlWithHttps = "https" + originalUrl.Substring(4);

Response.Redirect(urlWithHttps);

}

This code will replace the **http** prefix in the requested URL with an **https** prefix and redirect the user to new URL.