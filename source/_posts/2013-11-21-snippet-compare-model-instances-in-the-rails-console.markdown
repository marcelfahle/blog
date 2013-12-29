---
layout: post
title: "Snippet: Compare Model Instances in the Rails Console using sprintf"
date: 2013-11-21 15:02
comments: true
published: true
categories: ruby, rails, console, snippets
---
Today we had some technical problems with one user account on [Jumblzar](http://jumblzar.com)
(albeit our testing suite), and I wanted to check out this particular user and compare 
the individual attributes with a user that didn't have these issues. Since our User Model
has around 50 attributes, I wanted a side by side comparison with the working user.    

[sprintf](http://www.ruby-doc.org/core-1.9.3/Kernel.html#method-i-sprintf) to the rescue!   

{% gist 7587414 compare_jz_users.rb %}

I don't use sprintf in real life very often (even though I probably should), but here it came in handy. 
What I basically do is, I iterate over the attributes of one of my two user models, 
`u` (the working one) and `u2` (the faulty one).
I then throw the attribute name, as well as the assigned values of each user model into `sprintf`
and format the output with `"%26s - %42s  || %42s"`, which basically means "the first parameter 
you send me is a string and we make it 26 characters wide, and the other two parameters (the actual 
values) are gonna be strings each 42 characters wide". Et voilà, a nice side by side comparison 
of my user models in just a few minutes - here a little excerpt:

{% gist 8166475 sprint_list  %}

A side note: By default `Array#each` returns the original array at the end of the iteration, 
and since the console always returns the return value of the last command, 
we would see a whole lot of gibberish right after our nicely formated list. So instead of
feeding the console the original array from the iteration, I just add a `nil` after 
the block, so rails returns that. 

