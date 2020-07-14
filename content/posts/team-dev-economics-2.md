---
title: 'Team Dev Economics'
date: Sat, 12 Nov 2005 16:29:09 +0000
draft: false
tags: ['Misc']
---

**Background**

I used to be a programmer. I guess I liked the job, but I wasn't happy. I felt stuck. I wanted more control, I wanted to make all the decisions. I was sure I knew better than the rest of my teammates, and much better than my team lead. What can you do, I was twenty one. Then a funny thing happened: someone up there (where the air is thin, I guess) decided I should be promoted. So they promoted me to be the new team lead. I was happy and grateful.

The hardest two and a half years of my life followed. Being the team lead wasn't easy on me. At different times I was angry, stressed, pissed, hopeless, clueless, annoyed, betrayed, outraged, ashamed, bored. Most of the time I was more than one of the above at the same time. On a couple of oh-so-special thursday nights I was all of them at the same time.

I constantly felt like I'm about to have a heart attack, my ears will blow off my head to release all that steam, and my intestines will spill out of my eye sockets.

It was a blast.

Two weeks ago my time was finally up. I finished the job and returned to being a programmer for the two months remaining until I'm released.

I noticed two things about that two and a half years.

The obvious - I learned a \*lot\*.

And the not so obvious.

I learned how to be a better programmer.

**A good programmer**

Well, a definition is in place I guess. It's not enough to write fast and write without bugs (It's a must, but not enough).

The most important things to me in a great programmer are: (a) team work, (b) "big head", (b) getting things done.

(a) Any software today that's worthwhile (I mean, that can make profit) is a product of a team of people. That software and these teams succeed and fail as a team of **people**.

(b) Software development done by a team is almost totally unmanageable (don't beleive anything else they say). That doesn't mean there are no correct and "best" practices. If you don't do them - you're screwed for sure. But they don't guarantee success either. Once you have all the correct practices in place, you solely rely on the programmers to do all the needed things, big and small, in order for the project to be successful. And it takes a very special kind of person (a "big head" they call them in the [IDF](http://www1.idf.il/DOVER/site/homepage.asp)) to do these things. Trouble is, you can't really define these things in advance, and you can hardly teach anyone to do them.

(c) Programming is the perfect breeding-ground for perfectionism. Software can never be "done". There will always be unsolved bugs, and unincluded features. And this is just the excuse any programmer needs to not get things done. So when you realize that the project consists of a thousand tasks, all of which must be concluded for the project to be done, you must have programmers who know how to get each and every task done. Or your project is rotten eggs.

**Team Economics**

Your career, good programmers, bla bla bla. What's with the stuff about economics?

I know, I took the long rout, but I'm getting there.

See, Adam Smith teaches us that the [invisible hand](http://en.wikipedia.org/wiki/Invisible_Hand) guides us in all our actions, individuals and groups alike. And when everybody does what's best for them, the whole benefit. It's why Communism collapsed, America is the only super power, and Big Macs are so cheap and are everywhere.

![bigmac.jpg](/wp-content/uploads/2015/10/bigmac.jpg)

Now, these theories are applied to [more and more areas of life ](http://www.amazon.com/exec/obidos/redirect?link_code=as2&amp;path=ASIN/006073132X&amp;tag=bitzblog-20&amp;camp=1789&amp;creative=9325)lately. But I never though of it in relation to sofware development, nor have I read about such application anywhere. I should have.

Last week I was sitting at my desk, hacking away, when I had an idea of how to shorten our solution build times.

<side note (but wait, spaces are not allowed here! aaaaaa!)>

Compiling takes a lot of time at our place. We have some build-events inside Visual Studio that make it longer. I figured I could save myself a minute or so for each time I build the solution, by adjusting the build events. Turns out it was a stupid idea, embarassing even, so I won't go into it. The technicality of it is really not the issue here.

</side note>

I estimated I can save myself something like 30 minutes a day, if I invested a couple of hours immediately. I could be making "profit" within a few days. So I went for it. I implemented the solution, was happy, but then realized it was all unnecessary, so I threw it away. Case closed.

But this got me thinking about the way I made the decision to invest the time in implementing the time saving feature.

It was pure economics. Cutting build times had nothing to do with the specific coding task I was working on. And I was about to \*lose time\* working on something else. I still went for it, simply because I could see clearly that I will be able to produce more within a weeks time. See? I am on to something.

**Examples**

I'm sure a bell is ringing for you now. These kinds of decisions are everywhere in the lives of the project and the team. Usually the profit/loss calculation is more complicated.

I can develop a quick solution in an ASP.NET page which will benefit me know (code complete, go home early) or I can develop, say, a reusable web control which will take twice the time but will cut development time of future pages in half for everyone on my team.

I can develop a class that accesses a database table for my module or I can develop a general-purpose class that uses metadata files to access any table.

I can unit-test my code using a temporary driver page or develop a clean unit test class which will benefit me when I make changes to the code later.

And so on.

And how are these decisions usually made in a real-world team dev. environment? 99% of the time it is the developer measuring visible short time savings (say, in the couple-of-weeks area).

**So Now What? (A Word of Advice for Team Managers)**

The issue I just described is so ubiquitous, you can almost define our jobs as handling and managing this kind of economics well.

Look at the team as the single entity in this economy. Profit is less bugs, shorter schedule, \*as a whole team\*.

One developer working for 3 months, then 3 developers working for one week each is better than 4 developers working for 1 month each. It's not economical for the lone person working the longest, it's profitable for the team.

Single task taking 1 month, than 5 tasks taking 1 day is better than 6 tasks taking 2 weeks each.

How many of these decisions are made the "profitable" way for the team? Not many. Because the economics governing the single person who makes the decision are not the economics that benefit your team.

So it's your job to create an environment where the coder profit will coincide with the team profit.

How? I don't have non-obvious ideas yet. The obvious to me is: you must be aggressive in "investing" (usually time) correctly to make profit (usually later). Rule of thumb: if you're in a team that looks like the ones I know, you're probably not making enough of these investments. Your team mates are probably making most of the decisions, based on narrow single-coder economics.

**So Now what? (A Word of Advice for Programmers)**

We're all human beings. It's okay to be guided by self-benefit. Don't be your team's [communist](http://en.wikipedia.org/wiki/Leon_Trotsky), it won't work. But we're probably not making enough of the right investments either. Don't be short-sighted, many investments can bring profit in a short time. But you got to have balls, of course. Nobody cares what kind of stuff you developed if you took twice as long to accomplish your current task.

(1) Be confident. You'll make up the lost time before they'll fire you, most of the time. Pop quiz: anyone you know got fired for building solid frameworks? Reusable controls?

(2) Market yourself. Make sure everybody knows what you do. Make others benefit from your investments, you'll become indispensable. It's all enonomics after all.

**Conclusion**

Team dev. economics are an interesting way, IMO, to look at what we do, as team managers and developers. I didn't study economics unfortunately, but maybe some insights into our jobs will come from looking at them from an economical stand of point. I think I can do some thinking and googling about it now.