---
layout: post
title:  RPT - Modify Cookies using Java custom code
date:   2016-06-20 21:27:00 +0530
author: Arun
categories: RPT
---
### When to worry about cookies in playback
Cookies in HTTP are automatically taken care by RPT except once in a blue moon. It is only when the
client *(used while recording, typically a browser)* creates a cookie and places it in headers that RPT can't do it automatically. In other words, all
cookies sent by the server application using a set-cookie command on HTTP Response headers are dealt with by RPT
automatically and the rest aren't.

### How to modify cookies using custom code
Cookies used for playback are stored as part of the virtual user's data and can be accessed via RPT Custom code.
This [IBM technote](http://www-01.ibm.com/support/docview.wss?uid=swg21236931) explains how.
In addition, the IBM Knowledge Center has multiple RPT custom code samples and modifying cookies
is [one among them](https://www.ibm.com/support/knowledgecenter/SSBLQQ_9.0.0/com.ibm.rational.test.lt.common.doc/topics/rclearcookies.html?pos=2).

### What goes wrong commonly
In spite of valiant attempts, I've had instances in the past where my custom code didn't give expected results.
Regardless of the custom code, RPT sent the originally recorded cookie itself. I also recall multiple instances
where attempts to write a new cookie fails and RPT doesn't send the cookie at all.
In our experience so far, this happens because one or more of the cookie's attributes were incorrect.
For the cookie to get set properly, the cookie's attributes have to be correct. One reliable way to
do this is to make sure that the cookie attributes for the new or modified cookie matches what's in there
 for the other cookies on the same HTTP request. Watch out especially for the Path and Domain attributes.

### An example
One of the applications under test had playback failures which weren't getting sorted out though all dynamic
parameters in request URL and data were okay.
We found out that the application does something unusual or uncommon. The name of one of the cookies
is dynamic and is picked up from the response body of one of the previous requests. The cookie is not
being set using a set-cookie command. RPT was simply playing back using the cookie name and value as it
lies in the recording which the application quite obviously didn't like.

To circumvent this problem, I used custom code. The cookie name was sent into the custom code as an argument.
In order to identify the correct values of the Path and Domain attributes, I printed out all cookies available
on the virtual user's cache.

A gist of the custom code is [here](https://gist.github.com/arunkutty/3de470f6731cec61b4dfda55c0019529).
