---
title: 'Useful Math - Why the FIFA Ranking System Sucks'
date: Fri, 21 Nov 2008 15:55:06 +0000
draft: false
tags: ['Misc']
---

The [FIFA ranking system](http://en.wikipedia.org/wiki/FIFA_World_Rankings) is used to determine the seeding of national teams for various international competitions. It is considered absolutely broken by every football fan. For example, the Israeli national team is currently ranked 15th(1) in the world, while Sweden, for example, is only 29th. Enough said.

I decided to look into the method used by FIFA to determine the rankings, in order to understand why the system is so obviously broken and whether it can be fixed. To save you reading the detailed explanation in the Wikipedia article above, the method, in broad terms, goes like this:  
1\. For each game played by a given team, award the team a point amount using the formula:  
**Amount** = \[result points\] X \[match status\] X \[opposition strength\] X \[regional strength\].  
We'll look into the specifics soon.  
2\. Take then average of all the **Amount** values for all the games played by the team in the last year. This average is the score used to determine the rankings.  
(In fact, the last four years are taken into account, but this isn't important to our discussion)  
  
"Regional strength" doesn't have much effect relatively to the other factors, so lets ignore it.  
"Result points" is 3 points for victory, 2 for victory in penalty shootout, 1 for draw or loss by penalty shootout and 0 for defeat.  
  
So, where's the problem? The problem is the "match status" and "opposition strength" factors.

**Opposition Strength**  
  
Here's how the opposition strength factor is calculated:  
(200 - \[opposition team ranking position\]) / 100  
(If the result is less than 0.5 - the value 0.5 is used)  
The logic is this - there are about 200 teams in the ranking, so, when playing the best team, you'll get a strong bonus (200-1)/100=**2**. The bonus will be less and less significant the lower the opposing team is ranked, and starting at number 101, you'll actually be punished. For example for playing the 120th team the factor will be (200-120)/100=**0.8**.  

The problem is that this formula ignores a simple but important truth: the best teams are much much stronger than all the rest. There are about 20 national sides that are consistently reaching the knockout stages of important international competitions and providing the star players to all the top clubs.  
However, with FIFA's method, you will get a very high factor (of 1.6) for beating, for example, a team like Lithuania or Northern Ireland, ranked 41th and 42th respectively. These two teams aren't much better than South Africa (80th) or Austria (92th).  
A quick fix that comes to mind is to take the **square** of the formula. This will "spread out" the factor a bit. You will get a factor of ( (200-10)/100 ) ^ 2 = **3.61** for playing the 10th ranked team, but only ( (200-50)/100 ^ 2 ) = **2.25** for playing the 50th ranked team.

**Match Status**  
  
Match status is supposed to express the truth that a match that is played in an important competition like the World Cup is contested much more heavily than just a friendly game.  
The way FIFA expresses this in the calculation is to assign a value of 1 to a friendly, 2.5 to a qualification match, 3 for a regional cup (like the Euro) and 4 to the World Cup. This value is used as a factor in the multiplication used to calculate the score for a specific match played by a team.  
Lets assume that beating world champions Italy is worth 500 points. Then beating Italy in a Euro qualification match would be worth 1250 points and beating them in a friendly 500 points. So far so good.  
But remember that to calculate the rating FIFA takes the **average** of the points assigned to the team in the last 12 months. So, for example, if a team plays 3 matches, beating Italy once in a Euro qualification match and twice in a friendly, this team's rating for the year will be:  
(1250 + 500 + 500) / 3 = **750**  
However, if the same team beats Italy in two more friendlies in that year, the rating will go down to:  
(1250 + 500 + 500 + 500 + 500) / 5 = **650**  
Effectively, the ranking system punishes you for **victories** in friendly games.  
  
The correct way would be not using the match status in the points calculation **for a single match** but rather using it to calculate a **weighted average**. This way, in the previous example, the rating will not be hurt by beating one of the best teams in the world in a friendly:  
(500 \* 2.5 + 500 \* 1 + 500 \* 1 + 500 \* 1 + 500 \* 1) / (2.5 + 1 + 1 + 1 + 1) = **500**  
But also, an important match will have more influence on the overall rating. For example if a team plays 3 matches, with scores of 500, 700 and 700, when the latter two are friendlies and the former is a qualification match, the rating will be:  
(500 \* 2.5 + 700 \* 1 + 700 \* 1) / (2.5 + 1 + 1) = **588**  
Closer to the result in the important game than to the unimportant ones, as desired.

Unfortunately, I am sure the nice people at FIFA are wisely spending their attention on expensive women and liquor, instead of on silly math.