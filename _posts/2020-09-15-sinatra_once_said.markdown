---
layout: post
title:      "Sinatra once said, "
date:       2020-09-16 01:09:25 +0000
permalink:  sinatra_once_said
---


"You may be a puzzle, but I like the way the parts fit," and I can't think of a better sentiment for learning to build a web app using Sinatra (not the singer this time). 

Venturing into web apps and leaving the confines of the command line is like walking out of Plato's cave. Building my CLI at the end of unit one, I really felt like there was so much power in what I had learned (and there was), but getting to see how that power can be wielded in such a way as to allow discreet users and persistence of data has been exhilarating (I can only imagine what's coming)! 

The most notable difference between my experience building the CLI project and the Sinatra project is in how you route the user. It's obviously a very important aspect of any interactive application. You want to make sure your user knows what they can do, only does things that are good for them and for your program, and nobody gets hurt or lost around the way. In my CLI project, I was struck by just how much time I spent retracing my steps to make sure I had methods that would handle every possible scenario of user input. But in walks Sinatra and the wonderful magic of RESTful routes. Their purpose and usefulness were immediately apparent to me. Users have the illusion of freedom to move around your app in ways that feel very intuitive, and they don't ever have to really think about it.

This eliminates a good deal of retracing your steps, but the fact remains, it's still a bit of a puzzle. You have to make sure you have an appropriate controller for each of your models. You have to make sure your controllers are operating RESTfully, and without excess, unnecessary routes (in my case, not all models needed to be able to be deleted by the user), and you have to have appropriate views that can give your user a meaningful and coherent experience, all the while maintaining relationships and appropriate data in your database. It really did feel like a puzzle, but a good puzzle that you can't seem to put away.  

In the end, hopefully I was able to create a meaningful experience for my users in the finished application. I built an app that I have been looking for for a while: a simple, streamlined place to keep track of what books I have in my library and to take notes on them periodically. The HomeLibrary app does just that. It does not allow users to interact at this point as it is more of a journal than a blog, and allows users to organize their books by genre and keep notes on them as they read. I've already started cataloguing my library. It feels nice to already be making something useful with these new skills!

