---
title: 'Using Better Naming to Clarify Code'
date: Tue, 27 Dec 2011 12:34:39 +0000
draft: false
tags: ['Code', 'Misc']
---

.code{font-family:Lucida Console, Courier New;background:#ffdf73;}

I'm a big fan of good naming in code, here's a recent example:

Suppose you have a unique index in a database table and you're trusting that index to enforce no more than one record with the key.

So you're using an _insert ignore into...on duplicate key update_ statement.

So you end up calling something like _DataAccess.InsertRecord(data)_ or _DataAccess.AddRecord(data)_. Looking at such code it's very unclear that what really happens is an insert/update and you're only left with one record.

You can go the way of making your code explicit my moving the logic into your app and doing something like

var record=DataAccess.GetRecord(key);  
if(record == null)  
  DataAccess.InsertRecord(data);

But then you'll be losing the power of using the database do that for you.

So what I'm suggesting is just making your naming better, for example: _DataAccess.ReplaceRecord(data)_.