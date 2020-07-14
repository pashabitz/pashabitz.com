---
title: 'Solving the Measuring Paradox'
date: Thu, 07 Sep 2006 19:09:51 +0000
draft: false
tags: ['Misc']
---

Here's a look at how attempts to measure quality in three seemingly unrelated scenarios affect the results of what is measured, , and what can we do about it.

We'll start with measuring in the software development process. Joel [often claims](http://www.joelonsoftware.com/articles/fog0000000026.html) that introducing metrics into software development usually does not work. That's because programmers are smart enough to work around the metric, optimizing the specific thing that is being measured, with the end product not necessarily improving. For example: when you say that you measure programmers by the amount of bugs that are found in their code, they work out a way to get bug reports from QA without submitting them to the bug tracking software or just don't accept bugs as bugs instead of fixing them.

Where else does a similar situation arise?

**Example # 1 – Measuring the Risks of Cigarettes**

A recent Slate article, [Nicotine Madness](http://www.slate.com/id/2148748), discusses how measuring the levels of dangerous chemicals(nicotine, tar and others) in cigarettes, done by the U.S. government since the late 1960's didn't make the cigarettes any healthier, even though the government did punish makers with higher rates of dangerous materials.

The measuring strategy did succeed in one thing. **Making the measured properties of cigarettes lower**.

Here's why: the measurements are performed by a smoking-machine that simulates a human smoker. Yes, they stick the cigarette in and the machine starts puffing away, recording the levels of the dangerous chemicals. The problem is – it does not really simulate the way a human smoker smokes. Human smokers will smoke in a way that extracts the (ever growing) craved amounts of dangerous chemicals from the cigarette.

So what do the tobacco companies do? Hire the brightest scientists(they can afford it) to develop cigarettes that will score better on the machine tests but allow the smokers to inhale more and more nicotine and tar.

**Example #2 – Doping in Sports**

You have probably witnessed the hoopla surrounding use of illegal substances in sports. [Here are](http://www.cbc.ca/cp/sports/060822/s0822114.html)  [some examples](http://www.news.com.au/adelaidenow/story/0,22606,20034084-12428,00.html).

Am I the only one who feels it gets more and more common for top athletes to get caught using performance-enhancing drugs? I think not. But what else is growing at an even faster pace? You guessed it right – drug testing. The authorities that govern the various sports declared an all-out war on doping and drug testing became a fact-of-life for most (if not all) professional sportsmen. The punishments for those who break the rules get tougher all the time – multiple-year career-ending suspensions are now the norm.

Lo and behold: more and more athletes use the drugs. For every researcher that develops innovative drug tests, there are two who develop drugs that cannot be tested against. Once again, a measurement strategy backfires and makes the result even worse.

**![bolivia01.jpg](/wp-content/uploads/2015/10/bolivia01.jpg)**

**So Pasha, What to Do?**

First, I recommend you read an excellent book called [First, Break All the Rules](http://www.amazon.com/gp/redirect.html?link_code=ur2&tag=bitzblog-20&camp=1789&creative=9325&location=%2FFirst-Break-All-Rules-Differently%2Fdp%2F0684852861%2Fref%3Ded_oe_h%3Fie%3DUTF8). The authors suggest some uncommon knowledge ideas to get better results when managing people. One of them is **define the goal very well, let the people decide on how to reach it**.

This can also be a solution for our "Measuring Paradox":

The problem with measuring is not that it doesn't work. **It's that it works**. We just measure the wrong thing. What do we measure? We measure some properties that we suspect to be correlated with the desired result. A lower bug count is a sign for a program with few bugs. Lower nicotine levels are a sign for a healthier cigarette. More cheating athletes caught is a sign for a cleaner sport.

This approach doesn't seem to work.

Why don't we, instead of measuring properties that might lead to a better outcome and rewarding for better measurements, just **measure the outcome itself** and reward for a better one?

Specifically:

1\. Award the programmer with a bonus when the software makes money ("profit sharing"). Instead minimizing bug counts she will do anything to make the product a success.

2\. Award the tobacco companies when less people die because of smoking. For example – make them pay a special tax that grows with the number of people dying because of smoking-related reasons. Now instead of minimizing machine-tested nicotine levels they will do anything to make cigarettes healthier.

3\. Totally allow the use of performance-enhancing drugs, then integrate drug use in the result. For example: if you use drug A – 2 tens of a second are added to your 100 meters dash time. Now instead of thinking how to pass the drug-test, the athletes will concentrate on training for the best result. Using drugs will not give you an edge anyway.

The sad truth is that most metric-systems for performance are not very good at improving the outcome. So, the only thing you should really be measuring and trying to optimize is the desired outcome itself.

And don't smoke, it's not good for you.