---
title: 'Better Paradigm for AJAX Web Controls Design'
date: Sat, 14 Apr 2007 16:34:18 +0000
draft: false
tags: ['Misc']
---

**Summary:**

This one is not short. Better sit down.

I am going to discuss a common problem with web controls that are heavy on client-side javascript code and AJAX requests, and suggest a solution I came up with.

Right.

**An Example Control**

We'll be working with a sample control. Our control will be a stock ticker control that presents a current price for a stock and periodically gets price updates from the server.

Here's an example of such a thing, taken from Yahoo! Finance:

![Stock Ticker from Yahoo! Finance](/img/YAHOO_TICKER.gif)

You can see, that it displays the current stock price, a change in percent from some base price (opening price) and an arrow indicating the change trend.

It also highlights in green values that were recently changed (like the google stock in the screenshot).

(The coding examples are pretty sloppy. It's just an example, remember.)

The basic implementation allows setting the ticker to be displayed and renders a simple HTML table:

public  class  StockTicker : WebControl

{

> public  string Ticker
> 
> {
> 
> > get
> > 
> > {
> > 
> > > String s = (String)ViewState\["Ticker"\];
> > > 
> > > return ((s == null) ? String.Empty : s);
> > 
> > }
> > 
> > set
> > 
> > {
> > 
> > > ViewState\["Ticker"\] = value;
> > 
> > }
> 
> }
> 
> ![Code Snippet](http://www.pashabitz.com/content/binary/code_snippet01.gif)
> 
> private  int GetStockValue()
> 
> {
> 
> > Random r = new  Random();
> > 
> > return r.Next(1, 100);
> 
> }

}

We have a method GetStockValue that returns the current stock price. In our sample implementation it just returns a random value between 1 and 100. In real life it will probably access a database or use a webservice.

Lets test the control by placing it on a web form and running the application:

<%@  Page  Language\="C#"  AutoEventWireup\="true"  CodeBehind\="Default.aspx.cs"  Inherits\="SampleApp01.\_Default" %>

<%@  Register  Assembly\="SampleApp01"  Namespace\="SampleApp01"  TagPrefix\="cc1"  %>

<!DOCTYPE  html  PUBLIC  "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html  xmlns\="http://www.w3.org/1999/xhtml" \>

<head  runat\="server">

<title\>Untitled Page</title\>

</head\>

<body\>

> <form  id\="form1"  runat\="server">
> 
> <div\>
> 
> > <cc1:StockTicker  ID\="StockTicker1"  runat\="server"  Ticker\="CSCO"  />
> 
> </div\>
> 
> </form\>

</body\>

</html\>

We get a result similar to this:

![First Version of Ticker Control](http://www.pashabitz.com/content/binary/ticker_ver1.gif)

**Adding Dynamic AJAX Updates**

Now, lets add to our control the ability to receive dynamic price updates from the server using AJAX requests.

I will use the [Prototype javascript library](http://www.prototypejs.org/ "Prototype javascript library")[](http://www.prototypejs.org/ "Prototype javascript library") to simplify some client-side code.

Let's add a javascript file to the project (I named it StockTicker.js) and add the following code:

function requestPriceUpdateFromServer()

{

> var request = new Ajax.Request(
> 
> > "StockTicker.ashx",
> > 
> > {method:'get',parameters:'', onComplete:showResponse}
> > 
> > );

}

function showResponse(request)

{

> $('priceCell').innerText = request.responseText;

}

function periodicallyUpdatePrice()

{

> var periodicExecutor = new PeriodicalExecuter(requestPriceUpdateFromServer,3);

}

periodicallyUpdatePrice();

We are sending an AJAX request to to the URL StockTicker.ashx every 3 seconds. You can see that using the Prototype library it's literally a few lines of javascript.

Now let's add an HttpHandler to respond to the requests with the updated price of the stock:

public  class  StockTickerHandler : IHttpHandler

{

> public  void ProcessRequest(HttpContext context)
> 
> {
> 
> > context.Response.ContentType = "text/plain";
> > 
> > context.Response.Expires = -1;
> > 
> > context.Response.Write(StockDataProvider.GetStockValue("CSCO"));
> 
> }
> 
> public  bool IsReusable
> 
> {
> 
> > get
> > 
> > {
> > 
> > > return  false;
> > 
> > }
> 
> }

}

The request handler is very simple. You can see that I've refactored the stock data provider into a seperate class StockDataProvider as it is used in both the http handler and the control server code. Also, out of sheer laziness I have hard-coded the CSCO ticker into the parameter. In real life this should be passed from the client as a parameter of the AJAX request. If you run the example now, you will see that the price gets updated every 3 seconds, without re-sumbitting the page.

**Some Bells and Wistles**

Now let's add some logic to our control. First, after the price of the stock is changed, we will color green or red according to the change in the price (up or down). To achieve this, we modify the showResponse javascript function:

function showResponse(request)

{

> var newPrice = request.responseText;
> 
> var oldPrice = $('priceCell').innerText;
> 
> $('priceCell').innerText = newPrice;
> 
> if(newPrice>oldPrice)
> 
> > $('priceCell').bgColor = 'green';
> 
> else  if(newPrice<oldPrice)
> 
> > $('priceCell').bgColor = 'red';

}

The second addition will be to show the change in percent relative to the previous price.

We will modify in three places, first, communication between client and server:

1\. Pass the current price from client.

2\. Respond with the new price **and the change in percentage** from the server.

Second - calculate the precent of change on the server.

Thirs - display the change on client, after getting a response from the client.

Here's the modified code on client side:

function requestPriceUpdateFromServer()

{

> var data = $('priceCell').innerText;
> 
> var currentPrice = data.split('(')\[0\];
> 
> var request = new Ajax.Request(
> 
> > "StockTicker.ashx",
> > 
> > {method:'get',parameters:'price='+currentPrice, onComplete:showResponse}
> > 
> > );

}

function showResponse(request)

{

> var data = request.responseText.split('|');
> 
> var newPrice = data\[0\];
> 
> var oldPrice = $('priceCell').innerText;
> 
> var change = data\[1\];
> 
> $('priceCell').innerText = newPrice + '(' + change + '%)';
> 
> if(newPrice>oldPrice)
> 
> > $('priceCell').bgColor = 'green';
> 
> else  if(newPrice<oldPrice)
> 
> > $('priceCell').bgColor = 'red';

}

And in the http handler:

public  void ProcessRequest(HttpContext context)

{

> context.Response.ContentType = "text/plain";
> 
> context.Response.Expires = -1;
> 
> int oldPrice = Convert.ToInt32(context.Request.Params\["price"\]);
> 
> int newPrice = StockDataProvider.GetStockValue("CSCO");
> 
> double change = ((double)newPrice / oldPrice - 1) \* 100;
> 
> context.Response.Write(newPrice + "|" + change);

}

Now that we have a working and functional control, we can take a look at some problems with my implementation.

**What's Wrong**

I think the current implementation is fairly representative of how many modern working web controls are implemented. A major issue with this kind of implementation is the presence of **logic** on the client side and the mixing-and-matching of logic between server code and client code.

Let me expand on this issue. Here are some specific problems that we are lead to by this approach:

1\. **Logic** in javascript is bad on it's own right:

a) javascript is harder to develop and to debug - it is a more error-prone language than, for example, c#. It has weaker development tool support.

b) A lot of javascript logic code can lead to performance issues because it's executed on a potentially weaker, client-side, machine.

c) All javascript code is visible to the user, what is in many cases problematic because of security and intellectual property issues.

2\. Usually, the logic in your code will be scattered (just like in our example control) across server and client. This leads to:

a) Code that is very hard to understand and debug - there is no single place to look.

b) It's a horror to modify - notice how we had to modify many places when we wanted to show the change in percentage of the stock price.

At this point it's important to understand that I'm not bashing javascript code or the language as a whole. The language is extremely powerful and I like it personally. The browser support for javascript is actually what allows us to write truly powerful, usable and feature-rich applications on the web. So I do not, for a second, call to abandon it's use.

What **is** bad, is how **logic** gets tangled up inside javascript.

![Les Arcs Chairlift](http://www.pashabitz.com/content/binary/lift.gif)

**A Better Approach**

I have a suggestion for a better approach, that will keep the benefits of having controls that are rich on the client side but still have all the logic in one place, **on the server**.

The solution outline is this: we extract all the decision making from javascript and move it to the server while keeping the **performing** **of actions** on the client side. The server will decide what to do, and call (we'll see how in a second) the specific javascript actions that are needed.

First, let's map out all the things that are "logic" inside javascript in our current implementation: (logical decisions in bold)

1\. Immediately at the beginning of the page's "life on the client", javascript **starts a periodic request to the server for price updates**. This fact is a logic decision.

2\. It **sets the interval of updates to 3 seconds**.

3\. When requesting update from server, it **sends to the server the current price**.

4\. It knows the structure of the response from the server, **parsing it into (a)price and (b)change in percent, then injects these values in specific locations in the display**.

5\. It **checks the kind of change (up or down) and colors the display accordingly**.

Note how I am religious about this. We have to seperate every tiny bit of logic from the rest of the code.

Now, let's take a look at a better implementation where all the above logic parts are performed by the server.

Here's an implementation that removes the first three issues. The javascript:

function requestPriceUpdateFromServer()

{

> var request = new Ajax.Request(
> 
> > "StockTicker.ashx",
> > 
> > {method:'get',parameters:postBackParams, onComplete:showResponse}
> > 
> > );

}

function showResponse(request)

{

> var data = request.responseText.split('|');
> 
> var newPrice = data\[0\];
> 
> var oldPrice = $('priceCell').innerText;
> 
> var change = data\[1\];
> 
> $('priceCell').innerText = newPrice + '(' + change + '%)';
> 
> if(newPrice>oldPrice)
> 
> > $('priceCell').bgColor = 'green';
> 
> else  if(newPrice<oldPrice)
> 
> > $('priceCell').bgColor = 'red';
> 
> postBackParams = 'price=' + newPrice;

}

function periodicallyUpdatePrice()

{

> var periodicExecutor = new PeriodicalExecuter(requestPriceUpdateFromServer,interval);

}

You see that now, periodicallyUpdatePrice is not called right away from inside the javascript file itself. It also uses a variable for the interval of the periodic update requests. The call and the variable will be created by server-side code.

Also, requestPriceUpdateFromServer now uses a variable to hold the parameters to be sent to the server. It is also created by the server.

(showResponse still has knowledge of how this parameter is built, we'll remedy that in a sec.)

Now, let's take a look at the server:

private  int value = -1;

private  int GetStockValue()

{

> if(value==-1)
> 
> > value = StockDataProvider.GetStockValue(Ticker);
> 
> return value;

}

protected  override  void OnInit(EventArgs e)

{

> base.OnInit(e);
> 
> Page.ClientScript.RegisterClientScriptInclude("prototype", "prototype.js");
> 
> Page.ClientScript.RegisterClientScriptInclude("tickerScript", "StockTicker.js");
> 
> string periodicUpdateScript = @"var interval = "\+ periodicUpdateInterval + @"
> 
> > Event.observe(window, 'load', periodicallyUpdatePrice, false);";
> 
> Page.ClientScript.RegisterClientScriptBlock(this.GetType(), "periodic update", periodicUpdateScript, true);
> 
> Page.ClientScript.RegisterClientScriptBlock(this.GetType(), "postback params", "var postBackParams='price='+" + GetStockValue().ToString() + ";", true);

}

So far I only modified the web control code, not the HTTP handler.

GetStockValue is now used twice, so it caches it's value.

OnInit is where the interesting changes are made. It emits code that defines a javascript variable for the periodic request interval and also the initial call to periodicallyUpdatePrice (which will be done in the onload event handler in javascript).

It also creates the variable that holds the update request parameters, assigning to it it's initial value.

Now let's move to fixing the remaining issues. As a first step, I will refactor the javascript code into seperate actions, without shifting responsibilities to the server: (showing changed functions only)

function showResponse(request)

{

> var data = request.responseText.split('|');
> 
> var newPrice = data\[0\];
> 
> var oldPrice = $('priceCell').innerText;
> 
> var change = data\[1\];
> 
> displayNewValues(newPrice, change);
> 
> colorCellBasedOnChange(oldPrice, newPrice);
> 
> updatePostBackParams('price='+newPrice);

}

function displayNewValues(newPrice, change)

{

> $('priceCell').innerText = newPrice + '(' + change + '%)';

}

function updatePostBackParams(newValue)

{

> postBackParams = newValue;

}

function colorCellBasedOnChange(oldPrice, newPrice)

{

> if(newPrice>oldPrice)
> 
> > colorCell('green');
> 
> else  if(newPrice<oldPrice)
> 
> > colorCell('red');

}

function colorCell(color)

{

> $('priceCell').bgColor = color;

}

Now it's all set for a one-move, guilliotine-like shift of logic to the server.

Here are the changes to the HTTP handler:

public  void ProcessRequest(HttpContext context)

{

> context.Response.ContentType = "text/plain";
> 
> context.Response.Expires = -1;
> 
> int oldPrice = Convert.ToInt32(context.Request.Params\["price"\]);
> 
> int newPrice = StockDataProvider.GetStockValue("CSCO");
> 
> double change = ((double)newPrice / oldPrice - 1) \* 100;
> 
> string color = (change>0)?"'green'":"'red'";
> 
> string response = "displayNewValues("+newPrice.ToString()+","+change.ToString()+");"+@"
> 
> > colorCell(" + color + ");" + @"
> > 
> > updatePostBackParams('price='+" + newPrice + ");";
> 
> context.Response.Write(response);

}

The server decides on the logic that will be performed on client-side, and sends it to the client as a snippet of javascript to be injected.

All the client has to do is run the returned code:

function showResponse(request)

{

> eval(request.responseText);

}

Notice that because it's extremely uncomfortable to "embed" javascript inside a server language like c#, I made a point out of making the actions on the javascript all seperate and easy to call.

Also, you can see, that the **details of performing client-side work** are still all on client side. The only thing that moved to the server is the decision **what** and **how** should be done (specifically **function calls** and **parameters**).

**Conclusion**

Even though the presented implementation is still not polished, it shows how you can develop a client-heavy control or page, while keeping all logic in one place, where it's easy to develop and maintain.

Here's another analogy: the javascript part of your controls should be treated much like SQL embedded in the code. You can have logic in your SQL statements, but it's considered a well-known bad practice, for the same reasons I described here for javascript. The better approach is to have your SQL perform just the data access **actions** while the application code (which is in your chosen language) ties it all together and makes the logic decisions.