---
title: 'Apache with PHP on a Windows Machine'
date: Wed, 09 May 2012 21:04:05 +0000
draft: false
tags: ['Code', 'Misc']
---

_The program can't start because LIBPQ.dll is missing from your computer. Try reinstalling the program to fix this problem._

If you're getting the above error when starting Apache after installing Apache and PHP on your Windows machine, go to your PHP install directory (e.g. c:Program Files (x86)PHP) and copy the file libpq.dll into the bin directory under the Apache install directory (e.g. C:Program Files (x86)Apache Software FoundationApache2.2bin).