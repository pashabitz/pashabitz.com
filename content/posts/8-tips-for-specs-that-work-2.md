---
title: '8 Tips for Specs that Work'
date: Thu, 10 Feb 2011 11:39:35 +0000
draft: false
tags: ['Misc']
---

Here are some tips for creating more useful software specs that I gathered from my experience working on [Delver](http://www.delver.com/in/?invite=friends-and-family).  
Note: these are mostly relevant for specs that deal with user-facing features, but some apply in general.  
  

Tell a story
------------

Try to structure your document as a story that describes what the user does in the same sequence users are going to do it in the eventual system.  
This conveys the experience better than just describing different pages and functions as standalone objects.  
  

A picture is worth a thousand words
-----------------------------------

Include as many sketches of what the user sees instead of trying to describe it in many words. Things like interactions between elements and lists of fields are much clearer when looking at them the same way the user will.  
Include in your spec a sketch of every significant UI "state" the user will go through (I use [Balsamiq Mockups](http://balsamiq.com/) to create my sketches).  
  

Use nested sections to create structure
---------------------------------------

Structuring your document with hierarchical sections creates structure and helps navigating your spec when reading it repeatedly. The spec will be used as a reference for a while. For the first read - the story-like narrative is best. For subsequent reads - the ability to jump to the relevant part is more useful.  
If you're using Word - use the built-in "Styles" feature for the titles of your sections. This also creates a handy navigation and allows you to quickly create a table of contents if you want:  
  
![](/wp-content/uploads/2015/10/word-styles.jpg)  
![](file:///C:/Users/pasha/AppData/Local/Temp/moz-screenshot-2.png)![](file:///C:/Users/pasha/AppData/Local/Temp/moz-screenshot-3.png)![](file:///C:/Users/pasha/AppData/Local/Temp/moz-screenshot-4.png)![](file:///C:/Users/pasha/Pictures/word-styles.JPG)  
  

Start new sections on a new page
--------------------------------

It's better to start a new section on a new page (Word shortcut ctrl-enter). This makes it very easy on the eyes when trying to read just a particular section:  
  
![](content/binary/blog%20-%20structure%20of%20page.png)  
  

Focus on what's important by removing the tedious details
---------------------------------------------------------

By nature, the spec contains a lot of details. You want the person who reads your spec to be able to grasp the main ideas and the UX flows while not being distracted by the details. Later, you want the details to be available for someone who's interested in the details **for reference**, having already understood the flow and the main idea.  
You can achieve this by moving details like form field lists, message lists and long if-else logic, out of the main story and into the appendix of the document.  
  

Track changes
-------------

Add a changes table at the top of your document. In this table, list the date and main points of every change you make. Your spec will be adjusted during the review and development process. Documenting the main changes helps people who already read your spec once quickly understand what has changed since the last time they looked.  
  

Use conventions
---------------

Like in code, it helps to have a few common conventions in all of your specs. Usages of this are a specific font/background color for literal text that is displayed to the user, developer notes and so on.  
For example, I use the following standard for literal text:  

Hey user, welcome to the website!

  

Don't worry about DRY
---------------------

If you're a past/present hacker, you love the principle of DRY. Don't worry about it as much when writing your specs. It's ok to repeat yourself and describe the same thing twice in two different places if it helps the story-like nature of the spec and its clarity.