---
title: 'SSL in ASP.NET - Part I'
date: Sat, 03 Feb 2007 17:57:54 +0000
draft: false
tags: ['Misc']
---

SSL is the standard protocol to secure communications of web sites and applications. If you are developing your application using ASP.NET on a windows server, making the necessary configurations for SSL is not very difficult.

Unfortunately, while trying to accomplish this task at work, I discovered there isn't one good source of information to get the whole job done.

In this series of (about) three posts I will try to get you up to speed on everything you need to do and how it's done.

So, on to part one: "What is SSL and How Do We Create a Certificate?". Let's go.

**A Short Intro on SSL**

(Ugly oversimplification coming at you)

[SSL](http://en.wikipedia.org/wiki/SSL "SSL") is the standard protocol for (1)authenticating one or both parties and (2)encrypting communication between the parties.

Authentication is accomplished by a party presenting a valid certificate.

Encryption is accomplished in two steps:

1.  Negotiating encryption key(s).
2.  Encrypting communication using the negotiated key.

**Setting up SSL for an ASP.NET App**

The basic steps to do this are:

1.  Obtaining a certificate for use by the server.
2.  Configuring IIS.
3.  (Optional) Enforcing the use of SSL by creating a redirection mechanism in IIS and ASP.NET.

Usually, it's desired to create the setup at the development stage so you will be able to develop, debug and test your application in the same structure it will exist come production-time. So, in this post I will explain the whole setup for you developer station (referred to as localhost).

**Obtaining a Server Certificate**

What's a certificate? A certificate is simply a character string that contains the public key of some entitiy (like your server) signed by a third party. Presenting a valid certificate guarantees to the person who wants to communicate with you that you are who you claim you are (think **drivers license**).

To have SSL working in the real world you need to buy a certificate from an **Authorized Certification Authority** (CA) like [Verisign](http://www.verisign.com/ "Verisign"), for example. An authorized CA is an organization that is recognized ("trusted") by client software (the web browser you're using, for example). Continuing with the driver's license metaphor, the CA in that case is the state government that issues the license.

A verified certificate costs money and is only valid for the specific machine it was issued to, so for development and testing purposes you can create your own "self-signed" certificate.

On a side note, the fellows over at Verisign are basically charging people 1,500 USD for a short string in a file. Talk about a sweet and sustainable business model - no advertising revenue required, thank you very much.

**Introducing makecert.exe**

Luckily, on a Windows mcahine you can use the makecert command-line utility to create a self-signed certificate.

It has a s\*\*t-load of arguments, so here is the minimal command you need to run on the machine that hosts the application:

**makecert -pe -n "CN=localhost" -ss my -sr localMachine -sky exchange**

The interesting bits are:

*   \-n "CN=localhost" : creates the certificate suitable for use by server "localhost". Remember that every certificate is server-specific.
*   \-ss my : places the certificate in the "personal" certificates folder.

Last argument deserves a word: to view all certificates installed on your machine - use the Certificates MMC snap-in. Choose "local computer" and you are presented with various "stores" which are basically folders of certificates. To consume a certificate in IIS you need it to reside in the "personal" store of local computer.

There are additional useful arguments to the makecert utility, but the basic ones I presented will get you up an running. If you can see a certificate named "localhost" in your personal certificates store you are doing fine so far.

Stay tuned for the next part, where I will cover configuring IIS for SSL support using the certifcate we created.