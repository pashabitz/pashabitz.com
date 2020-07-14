---
title: 'Code Sample: Consuming the Twitter Streaming API'
date: Sun, 06 Mar 2011 15:48:48 +0000
draft: false
tags: ['Code', 'Misc']
---

.code {background-color: rgb(205, 205, 205); font-family: lucida console;} Here's a c# example of consuming the [Twitter Streaming API](http://dev.twitter.com/pages/streaming_api).  
Suppose you want to take all Tweets mentioning a country and save it to a database.  
I will be using the [Twitterizer c# library](http://www.twitterizer.net/). Let's first look at the complete code and then walk through it:  
  

private static bool streamEnded = false;  
public static void HandleStream()  
{  
  DateTime end = DateTime.Now + TimeSpan.FromSeconds(60);  
  
  OAuthTokens tokens = new OAuthTokens { ConsumerKey = "YOUR\_KEY", ConsumerSecret = "YOUR\_SECRET", AccessToken = "ACCESS\_TOKEN", AccessTokenSecret = "ACCESS\_TOKEN\_SECRET" };  
  TwitterStream s = new TwitterStream(tokens);  
  s.OnStatusReceived += new TwitterStatusReceivedHandler(s\_OnStatusReceived);  
  s.OnStreamEnded += new TwitterStreamEnded(s\_OnStreamEnded);  
  FilterStreamOptions ops = new FilterStreamOptions { Track = new List<string> { "italy", "germany", "spain", "france", "england" } };  
  s.StartFilterStream(ops);  
  
  while (DateTime.Now < end && !streamEnded)  
     Thread.Sleep(1000);  
   
  
  s.EndStream();  
}  
static void s\_OnStreamEnded()  
{  
   streamEnded = true;  
}  
static void s\_OnStatusReceived(Twitterizer.TwitterStatus status)  
{  
  SaveTweetToDatabase(status);  
}

Now let's look at the code in depth. First, as we'll be processing tweets in a loop, let's set an end to our loop. Let's say we want to process tweets for 60 seconds:

DateTime end = DateTime.Now + TimeSpan.FromSeconds(60);

Next, we setup the Twitterizer class used to handle the Streaming API:

OAuthTokens tokens = new OAuthTokens { ConsumerKey = "YOUR\_KEY", ConsumerSecret = "YOUR\_SECRET", AccessToken = "ACCESS\_TOKEN", AccessTokenSecret = "ACCESS\_TOKEN\_SECRET" };  
TwitterStream s = new TwitterStream(tokens);

The class TwitterStream is part of the "addons" to Twitterizer, so you will need to download the source of the [2.3.2 release of Twitterizer](http://www.twitterizer.net/files/Twitterizer2.3.2-source.zip) and make a small fix as [I described here](http://www.pashabitz.com/2011/02/07/Twitter+API+Net+Libraries+Roundup.aspx).  
You will pass to TwitterStream the app keys of your Twitter application which you [set up here](http://dev.twitter.com/apps) and the OAuth tokens.  
Next, we set up event handlers for the stream of tweets:

s.OnStatusReceived += new TwitterStatusReceivedHandler(s\_OnStatusReceived);  
s.OnStreamEnded += new TwitterStreamEnded(s\_OnStreamEnded);

s\_OnStatusReceived will be called by TwitterStream when a tweet is received from Streaming API and s\_OnStreamEnded will be called when the connection to Twitter ends.  
Next, we may add some options to the Streaming API. There are [several methods to access the Streaming API](http://dev.twitter.com/pages/streaming_api_methods): you can either get tweets for a list of keywords, a list of users or a random sample of all tweets. In our example, let's ask Twitter for tweets mentioning a country and open the connection:

FilterStreamOptions ops = new FilterStreamOptions { Track = new List<string> { "italy", "germany", "spain", "france", "england" } };  
s.StartFilterStream(ops);

The last thing to do is wait for the amount of time we decided on in the beginning and then close the stream:

while (DateTime.Now < end && !streamEnded)  
     Thread.Sleep(1000);  
   
s.EndStream();