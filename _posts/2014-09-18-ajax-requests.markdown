---
author: Arun Raj
comments: true
date: 2014-09-18 12:30:54+00:00
layout: post
slug: ajax-requests
title: 'Ajax Requests '
wordpress_id: 77
---

AJAX stands for Asynchronous JavaScript and XML. A nice description about what Ajax is and how it operates is given in the [codeproject website](http://www.codeproject.com/Articles/31155/Ajax-Tutorial-for-Beginners-Part). The basic operation of Ajax can be understood from the figure below.


![AJAX OPERATION](http://www.codeproject.com/KB/ajax/AjaxTutorial/AJAX2.JPG)Again courtesy to codeproject.com


The codes below are some basic one's that I've tried while learning the fundamentals of ajax

**GET alert using ajax.**


```
function Alert(params) {
var url="http://localhost:59741/api/";//Define url for your api here
xmlhttprequest = new XMLHttpRequest();
xmlhttprequest.onreadystatechange = function () {
if (xmlhttprequest.readyState == 4 && xmlhttprequest.status == 200) {
alert(xmlhttprequest.responseText);
}
}
xmlhttprequest.open("GET", url+ params, true);
xmlhttprequest.send();
}
```


above Alert function accepts parameters which refer to api endpoints to do a normal get operation from the webservice

**POST using Ajax**


```
function PostDetails(params) {
var url="http://localhost:59741/api/";//Define url for your api here
xmlhttprequest = new XMLHttpRequest();
xmlhttprequest.onreadystatechange = function () {
if (xmlhttprequest.readyState == 4 && xmlhttprequest.status == 200) {
alert("Post Success");
}
}
xmlhttprequest.open("POST", url+params, true);
xmlhttprequest.setRequestHeader("Content-Type", "application/json"); // I'm using JSON format in this case
xmlhttprequest.send('<json data>');//fill the data in JSON here
}
```
