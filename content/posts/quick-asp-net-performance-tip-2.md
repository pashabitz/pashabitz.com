---
title: 'Quick ASP.NET Performance Tip'
date: Thu, 01 Sep 2005 19:21:42 +0000
draft: false
tags: ['Misc']
---

Setup:

1\. You have a page with a data-bound DataGrid.

2\. The page makes multiple post-backs (for example, the DataGrid is editable).

3\. There are template-columns in the DataGrid.

Here's the tip:

Make the ID of the DataGrid and the ID of the controls **inside** the template controls **shorter**.

See, the DataGrid control renders all the HTML elements it needs to display your data, and assigns them id's based on the ID of the grid itself and the ID of the controls they represent.

Have shorter ID's and the eventual HTML page that travels to the user will be much lighter. How much? We recently saved between 20% to 30% in a page that used to weigh in at around 500KB, and that's a lot.

Consider the following simple example:

I have a grid with two data-bound columns. The ID of the grid is "CitrusFruitData". Each column is a template-column. The item template of both contains just one control - an asp:label with and ID of  
"Label1".

If I bind 100 records to it, the output page is 36343 bytes long.

View source, and you'll see the something like the following (about 200 times...):

<span id="CitrusFruitData\_\_ctl7\_Label1">... 

Now if I shorten the ID's of the grid (to "Cf"), and the two labels in the item template of the two columns (to "L1"), the resulting output shrinks to 32930, which is an easy 9% improvement.

And when you have more columns, and more controls in the columns, and more rows in the grid – the improvements will be more dramatic.

But wait, crazy man! What about readability?

True, your code will be less readable, but this is one case when I will prefer the performance over readability. And the readability can be easily contained, if you're smart about it.

Good luck.