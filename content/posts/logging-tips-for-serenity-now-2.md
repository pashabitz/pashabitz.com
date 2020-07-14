---
title: 'Logging Tips for Serenity Now'
date: Thu, 22 Jan 2009 13:58:45 +0000
draft: false
tags: ['Misc']
---

There are few reasons to have your code log extensive error/debug messages. One of them is to help with debugging issues on a remote (or worse, production) environment. Here are some tips to help you get the most out of this kind of logging:

#### Log Actual Values

With a well placed log message you will be able to easily pinpoint the line of code where the problem occurred. Now you're left staring at that line of code and trying to guess what caused the error. This will be so much easier if you include in the log message the actual run-time values that your code worked with. This includes stuff like the parameters to the method, fields of the class, session values etc.

#### Log at Component Boundaries

A component can be a different .net assembly, code that resides in a different logical layer of your application, a third-party library or a piece of code you communicate with using anything than just a method call (like HTTP or WCF). Wherever there's communication between two components, there are bound to be "wiring bugs". Because most components are well thought-out and tested on a **standalone basis**, it is very useful to know what data was exchanged to help with debugging the issue. Always log all parameter values, both on the client (caller) side and the server (called) side.

#### Log at Organization/Responsibility Boundaries

This is a slight variation on the previous suggestion. Whenever a call is made to a piece of code owned by another person or group, you should log all parameter values. When you walk across the hall to ask the owner of that code about the bug you're trying to fix, the first thing they'll ask you is what are the exact values you're sending in.