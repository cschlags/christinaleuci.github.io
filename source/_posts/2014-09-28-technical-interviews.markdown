---
layout: post
title: "Technical Interviews, Oh My!"
date: 2014-09-22 09:21:02 -0400
comments: true
categories: [Technical, Advice]
---

So far my mentorship  at Stack Exchange has been amazing. I’ve been working with Max Horstmann, a Senior Developer on their Careers 2.0 team. We’ve mainly been going over mock interviews and how I should properly conduct myself in interviews. Of all the information I’ve taken away from our meetings has been the process for dealing with technical practices. As Max has said, “Repeat, Clarify, make examples, pseudo-code, code, and finally run it”.
<!--more-->

Many times an employer will give you a general question, check to see if a point falls inside a box. Now this may seem like a very simple question, just draw a box and see if the point falls inside those parameters; sadly it’s a little more complicated. One of the first things you should do is repeat the question to make sure you have haven’t missed anything. Many times you may mis-hear the employer and do something completely different than what you were expected to.
Clarify by checking to see if you’re taking in user data, and if you need to check to see if that data is correct. Another good question would be to check if “a point inside a box” means that the point falls on a box’s line or literally inside the lines. Lastly, another good question is if the box follows a simple x&y graph. It’s really helpful to write down these tidbits of information. By clarifying with your interviewer you show that you’re not just a robot, you’re capable of finding the loopholes inside a problem before coding. Generally an employer is just trying to gage your intelligence and won’t make it too complicated but it’s still smart to ask these questions and not get caught clueless.
Once you’re positive of the question and what path you should follow, you should begin making examples. If we’re following the problem earlier of a point inside a box, one path you could take is drawing an x&y graph with a box. A quick aside: if they are asking for user input, yes it would be simple for you to ask for each point of the box but would it be as simple for the user? NO.
In this situation the simplest group of inputs would be two diagonal points, for example let’s pick the bottom left and the top right points. This way you know the dimensions of the box by using basic math with the points.
Now that we have a diagram of the box and it’s points finished let’s move through the example. Let’s say we have the top right point as (8, 9), the bottom right point as (2, 1), and the singular point as (4,5). Your drawing should look like this:

<img src="{{ root_url }}/images/box.jpg" width="90%"/>

Now it’s obvious by your drawing that this point is inside the box, but your computer where you’ll be coding doesn’t know. What you’ll need to do is use simple math to see if this point (4,5) falls inside the dimensions of this rectangle.

<img src="{{ root_url }}/images/psuedo.jpg" width="90%"/>

Here you can see that you do solve the problem and that it gives the correct answer. What you need to do now is put it into code that can be used. If you’re using Ruby like me, then simply create a new class, that can be instantiated in your IRB and create a method that you can plug your data into.

<img src="{{ root_url }}/images/quicky.jpg" width="90%"/>

And you’re finished! Although the question may seem simple, there is a complicated process to it. At this point what you could do is, further optimize your code or run it to see if it works. If we were to optimize this could what we could do is take out the if statement. By doing something like this:

<img src="{{ root_url }}/images/final.jpg" width="90%"/>

it shows that you can solve a simple problem while optimizing your code.
Remember to check your code. It’s impressive to interviewers if you can catch your mistakes before presenting your code. Now at this point, an interviewer will either tell you good job or they will ask if it works. If they ask you the latter the best thing you can say is “I think it does, let’s check”. This shows confidence and that you’re willing to show you’re work.
If for some crazy reason your code doesn’t work, don’t worry, continue working on it and perfect it. Just rinse and repeat, if you can understand why it’s not working then you interviewer will still be impressed.
Congratulations you’ve completed your interview! Good luck to anyone who is hoping to do a technical interview! 