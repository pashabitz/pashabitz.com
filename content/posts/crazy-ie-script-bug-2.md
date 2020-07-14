---
title: 'Crazy IE Script Bug'
date: Tue, 12 Jul 2005 20:19:54 +0000
draft: false
tags: ['Misc']
---

I knew I didn't want to go to work this morning for a reason.

Well, aside from the fact I only went to sleep at 2am, my eyes were burning as if I just spilled some special gazoline-based, industrial-strength stain-remover on them and rubbed them with garlic, and I just all-around don't get turned on by going to work in the mornings. Never mind that.

I knew I had \*The Bug\* to deal with, and no matter how much I wished I had something super-important to do all day, I knew I had to tackle that bug \*today\*.

And I'm not talking with you about one of those well-behaved Im-getting-an-8-instead-of-a-7-return-value, school children type of bug. This one was a mean mother.

Well, there's this main page on one of our secret web-apps, and on it, there's some secret client-side javascript code that's supposed to run when the .aspx page finishes loading, and do a bunch of secret stuff.

It all works fine when run from localhost, but then when run from the test server, two times out of three it just doesn't run. And Shani, who's responsible for maintaining it, is, well, unavailable at the time, so I had to figure it out.

It usually takes 20 minutes to figure out you don't have a clue how to fix a bug. I did my 20 minutes a couple of days ago, so I wasn't very optimistic.

Well, I started with some debug statements.

The page has an HTC behavior attached to it, like this:

<body style="behavior:url('file.htc')" ...>

Inside the behavior file there's a function, with some code in it(!):

function foo(){  
//...  
}

Which is attached to the onreadystatechange event of the document (using an ATTACH tag in the behavior).

Function foo() checks for the readyState property of the document and when it's value is "complete" - does it's thing. And like previously explained, the thing doesn't get done for no apparent reason.

Like I said, after some debug messages(actually they were just output to the window.status, javascript alert's are just too damn annoying when trying to debug), I narrowed it down to:

1\. Either foo(), for some reason couldn't attach to the onreadystatechange event, some of the time,

2\. Or, the event wasn't even raised.

After some more time, I realized the event wasn't being raised at all, "2" was the correct one. Great. Some more time, and I figured out also that the document was "stuck" with readyState="interactive", and the body element(the one the behavior is attached to) with readyState="loading". Plus, the behavior file wasn't being loaded at all - even code placed inside it, but outside all functions, wasn't running. More proof to the theory, yet no advance towards the solution, getting home, being happy, etc.

So this was now my great question: something was causing the IE to be unable to load the behavior and because of that, not to finish loading the whole page, getting stuck in the same incomplete readyState, not throwing the onreadystatechange event and being generally miserable. Hmm. That's not even a question, is it? Right. The question was: Why?

Now, on a side note, all this probably doesn't sound much, but by this time I was way past my lunch break, pretty angry, and above all - clueless.

I managed to pass some time using excessive coffee making and consuming, cooler-talk and staring at my inbox. So I reached 16:45, time for my weekly weights training and so we, Doron and myself, went.

Coming back to the office I only hoped to be home before midnight.

Fortunately(for me then, and for you'all now), the solution came in about 90 seconds.

I noticed two script elements referring to external javascript files, like so:

<body...>  
<script language=javascript src="file1.js"\></script>  
<script language=javascript src="file2.js"\></script>  
<form ...>  
...

I remebered Shani saying something about placing stuff within the <head> element if you want to make sure it gets loaded fine. So I gave it a try and moved the two script's inside the <head>.

Voila. No problem.

Tour-de-France recap starts in two minutes, so quick concluding thoughts:

1\. If you can avoid client-script in IE, especially event-based, do it at any cost.

2\. Man, I'm happy it ended the way it did.

3\. To tell you the truth, I have no idea how to explain exactly what happened, why IE had trouble to load the script's and was getting stuck on the whole page. And how come it's so hot in here lately, for that matter.