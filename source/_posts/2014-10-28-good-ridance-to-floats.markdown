---
layout: post
title: "Good Ridance to Floats"
date: 2014-10-28 23:14:56 -0400
comments: true
categories: [Technical, Front-End]
---

As much as I have liked using floats in the past I have to recognize that there are much better substitutes to floats out there. Much of the Front-End programming world right now: 
<!--more-->

{% img center /images/floatsmeangirls.png 'image' 'images' %}

Not so long ago programmers mainly used HTML tables to design complicated websites. Over the years the up and coming CSS has taken a hold of the layout scene. One of the most popular and most difficult positioning properties to master are floats. Many programmers can surely admit that although they have become accustomed to using floats in my css files they are buggy, awkward, and pretty difficult to learn.

##What Are Floats?
Let's do a quick overview of what floats are. Floated elements are a part of the web page, they are different than page elements that use absolute positioning. Absolute positioned elements are removed from the flow of the webpage, this means that they no longer affect other elements regardless of if they touch.

There are four valid values for the float property. The default property is None, this means that the element will not float. Inherit will assume the float value of it's parent element. Left and Right are directional float values.
{float properties}

##Clearfix to the Rescue
The problem with floats is that they don't work well with their container elements. If you want your parent element to look as if it's holding floated elements what you need to do is put a "clear:both" inside the container on the last line. 

Of course programmers have hacked this nuance. Instead of possibly having single-lined css classes for parent elements to show their floated children.

Floats aren't entirely bad, they are fantastic at wrapping text around images so that the text does not overlay on top of the image. They have been used in creating large website layouts to smaller instances where reflow is needed.

##Inline-Blocks





