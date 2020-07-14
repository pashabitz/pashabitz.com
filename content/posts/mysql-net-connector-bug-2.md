---
title: 'MySQL .Net Connector Bug'
date: Sat, 20 Oct 2007 13:55:38 +0000
draft: false
tags: ['Misc']
---

I've stumbled upon (what appears to be) a bug in MySQL .Net Connector (version 5.1.3).  
I opened the bug in the MySQL bugs database, you can read the details here - [bug #31617](http://bugs.mysql.com/bug.php?id=31617).  
  
Basically, when calling MySqlCommand.ExecuteReader with an sql statement that times out - you get back a **closed** MySqlDataReader and the underlying MySqlConnection becomes corrupted, meaning you cannot use it for new operations and you cannot close it either.  
  
The workaround you can implement in the meanwhile is:  
  
1\. Always check that the reader you got is open before iterating on it:  

using(MySqlDataReader reader = command.ExecuteReader())  
{  
  if(!reader.IsClosed())  
  //use reader  
}

  
  
2\. "Fix" the connection by forcefully setting the problematic field using reflection:  

  //if we got a closed reader from MySqlCommand.ExecuteReader, need to set the Reader property of connection to null  
  PropertyInfo p = connection.GetType().GetProperty("Reader", BindingFlags.NonPublic | BindingFlags.Instance);  
  p.SetValue(connection, null, null);

  
  
  
**More on MySQL at pashabitz.com:**  
[MySQL dates quiz](http://www.pashabitz.com/PermaLink,guid,5bffe1b0-3de2-4fbd-9ae4-8bc999b1e9dd.aspx)  
[MySQL dates answer](http://www.pashabitz.com/PermaLink,guid,792ca9f2-7946-4264-9c1b-f890778bd20f.aspx)