---
title: 'Make the Ugly Scrollbars on your Facebook App Disappear'
date: Mon, 26 Dec 2011 19:50:42 +0000
draft: false
tags: ['Code', 'Misc']
---

If you have annoying scrollbars around your Facebook app's canvas, here's what you need to do:

First, go to your app's settings, click "Edit App" and then "Advanced" on the right.

Scroll all the way down to "Canvas Settings" and change "Canvas Height" to "Settable".

Second, add a call to **FB.Canvas.setSize()** ([http://developers.facebook.com/docs/reference/javascript/FB.Canvas.setSize/](http://developers.facebook.com/docs/reference/javascript/FB.Canvas.setSize/)). Make this call inside **window.fbAsyncInit**, after calling **FB.init**.

That's it! Gorgeous app, no scrollbars!