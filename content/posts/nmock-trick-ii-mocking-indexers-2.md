---
title: 'NMock Trick II - Mocking Indexers'
date: Thu, 15 Mar 2007 15:20:14 +0000
draft: false
tags: ['Misc']
---

Here's another short trick for the [NMock mock objects framework](http://nmock.org/ "NMock mock objects framework"):

To mock an [indexer](http://msdn2.microsoft.com/en-us/library/2549tw02.aspx "indexer") use the syntax (for the getter):

Stub.On(...).**Method(****"get\_Item"****)**.Will(Return.Value(..));

And for the setter:

Stub.On(...).**Method(****"get\_Item"****)**.Will(Return.Value(..));

**\*Update\***

Via [Paul Pierce's post](http://paulpierce.co.uk/?p=4 "Paul Pierce's post") I found a better way:

Stub.On(...).**Get\[****...****\]**.Will(Return.Value(..));