---
title: 'MySQL: Slow Query When Joining with a Union'
date: Mon, 29 Oct 2007 19:10:38 +0000
draft: false
tags: ['Misc']
---

The MySQL query optimizer has trouble with queries that contain a join to a union.  
Lets take a look at the following example:  

select u.f1, u.f2 from  
(select t1.f1, t2.f2 from t1 join t2 on t1.f3 = t2.f3  
union  
select t3.f1, t2.f2, from t3 join t2 on t3.f3 = t2.f3) u  
where u.f2 = ?val

  
  
Now, suppose we have an index on column f2 in t2. If you look at the explain plan, you will see that MySQL **does not** use it.  
So, the solution is to move the where constraint into the parts of the union itself:  

select u.f1, u.f2 from  
(select t1.f1, t2.f2 from t1 join t2 on t1.f3 = t2.f3  
**where t2.f2 = ?val**  
union  
select t3.f1, t2.f2, from t3 join t2 on t3.f3 = t2.f3  
**where t2.f2 = ?val**) u

  
  
This time, the index on f2 will be used and the query should perform much better.  
  
**More on MySQL at pashabitz.com:**  
[MySQL .Net Connector Bug](http://www.pashabitz.com/PermaLink,guid,a6ecf9f5-99c0-4c5d-81bd-56b752c33354.aspx)