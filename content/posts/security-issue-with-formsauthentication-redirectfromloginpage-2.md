---
title: 'Security Issue with FormsAuthentication.RedirectFromLoginPage'
date: Mon, 30 Apr 2007 21:13:43 +0000
draft: false
tags: ['Misc']
---

Here is a major security vulnerability in applications that use the [ASP.NET forms authentication](http://msdn2.microsoft.com/en-us/library/aa480476.aspx "ASP.NET forms authentication") mechanism.

Forms authentication exposes a configuration property called enableCrossAppRedirects. It's default value is false.

However, a simple test showed that this property does not have the desired effect, and it is possible for an attacker to redirect a user to a malicious website from your legitimate login page.

Assuming your login page is at **http://www.myapp.com/login.aspx**, and login.aspx uses the FormsAuthentication.RedirectFromLoginPage method, the following request will redirect the user to another domain after passing authentication by your application:

**http://www.myapp.com/login.aspx?ReturnUrl=http%3a%2f%2fgoogle.com%5c**

Although this is not an issue on it's own, it can potentially lead to serious security threats to your users in the form of information stealing attacks.

Another annoyance, is what the MSDN has to say about this. The [RedirectFromLoginPage method page in MSDN](http://msdn2.microsoft.com/en-us/library/1f5z1yty(vs.80).aspx "RedirectFromLoginPage method page in MSDN") has a specific note on the potential risks of setting enableCrossAppRedirects to true:

_Security Note_

_Setting the **EnableCrossAppRedirects** property to **true** to allow cross-application redirects is a potential security threat. When cross-application redirects are allowed, your site is vulnerable to malicious Web sites that use your login page to convince your Web site users that they are using a secure page on your site. To improve security when using cross-application redirects, you should override the **RedirectFromLoginPage** method to allow redirects only to approved Web sites._

Well, like we saw, you don't have to set the property to true. It works the same way when set to false.

But by far the most frustrating is

_... You should override the RedirectFromLoginPage method to allow redirects only to approved Web sites._

Hmm. Right.

Too bad that the FormsAuthentication class is **sealed**.

And that RedirectFromLoginPage is **static**.

**Real Solution**

The only workaround, in case you really want to disable cross-domain redirects, is to check the ReturnUrl query string parameter in your code, **before** calling RedirectFromLoginPage.