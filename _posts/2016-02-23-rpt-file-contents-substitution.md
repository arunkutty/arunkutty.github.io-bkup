---
layout: post
title:  RPT - File Contents Substitution when content is binary
date:   2016-06-23 23:13:00 +0530
author: Arun
categories: RPT
---
If you have a test case involving a file upload where a different file has to be uploaded from the system while playing back, what would you do?
If you think custom coding is required, you're wrong. RPT provides a simple feature to upload a different
file by leveraging the datapool mechanism. This [technote](http://www-01.ibm.com/support/docview.wss?uid=swg21424328) explains how.

As you can see in the technote, it's almost as easy as substituting from a datapool.
i.e Highlight content, right-click and say substitute. However, There is a small catch if the file contents are binary. The test editor doesn't allow selection of content.

In order to get it to work, you have to first "transform" the binary content into ASCII. The following steps tell you how.

First, locate the request where the binary data chunk for the file is present in the POST data. Right-click on the chunk and select "Apply transformation"

<img src="{{ site.url }}/assets/file-contents-substitution-1.jpg" alt="Transforming binary data contents">

And then in the pop-up box, select the transformation - "Convert to ASCII text".

<img src="{{ site.url }}/assets/file-contents-substitution-2.jpg" alt="Conversion to ASCII text">

You can now see the data content in ASCII form though it is unreadable. But at this point, the test editor allows you to select the content. you can select the whole content and substitute as described in the
[technote](http://www-01.ibm.com/support/docview.wss?uid=swg21424328).

