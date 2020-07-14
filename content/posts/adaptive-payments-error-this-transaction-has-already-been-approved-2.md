---
title: 'Adaptive Payments Error This transaction has already been approved'
date: Sun, 18 Dec 2011 11:09:59 +0000
draft: false
tags: ['Code', 'Misc']
---

If you're using the PayPal Adaptive Payments API in sandbox mode, redirecting the user to https://www.sandbox.paypal.com/cgi-bin/webscr and getting the following error:

> This transaction has already been approved

The problem may be that you're using an incorrect sandbox user as the "sender" in the transaction.

You need to go to the sandbox ([https://developer.paypal.com/](https://developer.paypal.com/)) and then to "test accounts" on the left.

You need to create a test account. Click "preconfigured" next to "new test account" and then make sure you select "buyer" under "account type".

Now use the email of this account that you've just created as the "senderEmail" in the call to the "Pay" API operation.