---
layout: post
title: "Snippet: Compare Model Instances in the Rails Console"
date: 2013-11-21 15:02
comments: true
categories: ruby, rails, console, snippets
---
Today we had some technical problems with one user account on [Jumblzar](http://jumblzar.com),
albeit our testing suite, and I wanted to check out this particular user and compare 
the individual attributes with a user that didn't had the issues. Since our User Model
has around 50 attributes, I wanted a side by side comparison with the working user.    

[sprintf](http://www.ruby-doc.org/core-1.9.3/Kernel.html#method-i-sprintf) to the rescue!   

{% gist 7587414 compare_jz_users.rb %}

In this example the variables `u` and `u2` 

