---
layout: post
title: "Custom compilers with CodeKit"
date: 2012-11-26 12:49
comments: true
categories: [haml, sass, less, tools] 
---
Recently I needed to do some static HTML stuff and wanted to use [Haml](http://haml.info/) for this. 
[LiveReload](http://livereload.com/), which I'm using since quite some time, lets you setup 
watchfolders and compiles all kinds of stuff for you like [CoffeeScript](http://coffeescript.org/), 
[LESS](http://lesscss.org/), [Sass](http://sass-lang.com/) and [Haml](http://haml.info/). 
Unfortunately it would only accept the hash syntax used in ruby prior 1.9 and that's because 
LiveReload uses my system ruby by default, which is 1.8.7. 
``` ruby shit adds up
%html{lang: 'en'}
vs
%html{:lang => 'en'}
```
It gives you the option to switch to another ruby version, but doesn't
support installations via rbenv (it will probably be introduced in version 3 of LiveReload). 
I read somewhere that RVM is supported, but I can't validate this.

Long story short, I needed a different compiler to take advantage of the new syntax. Since I 
like the watchfolder simplicity of LiveReload, I took a look at [CodeKit](http://incident57.com/codekit/).
From what I can say right now is, on the compiler end it does basically the same thing as LiveReload 
(please correct me if I'm wrong, I'm only using it for like 5 seconds), but offers a bit more flexibility 
when choosing your compiler. In my case I just went into the settings for Haml and pointed to the 
Haml binary inside my latest ruby installation (~/.rbenv/versions/1.9.3-p194/bin/haml), et voilà: Ruby 1.9 syntax in Haml.

{% img center /media/images/codekit-haml.jpg 648 481  %}

