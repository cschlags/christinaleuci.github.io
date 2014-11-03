---
layout: post
title: "Add Comments To a Github Blog"
date: 2014-10-28 23:14:56 -0400
comments: true
categories: [Technical, Github]
---

Have you ever wanted to add comments to you github pages blog? It will take a quick two minutes and you'll be ready for feedback!
<!--more-->

Today we'll be using <a href="http://disqus.com">Disqus</a>, this is something that's already implemented in the current update of Octopress. The first thing you'll need to do is signup for a Disqus account. If you already have an account you can skip down to <span style="font-weight: 500">Step 2</span>.

## Step 1
Once you've signed up for your Disqus account go to the icon at the top right of your page. In the drop down menu click on the My Home button.

<img src="{{ root_url }}/images/start.png"/>

## Step 2
At this point you will have come to your Disqus homepage, for your purposes of using Disqus as a comment feed you won't need the homepage much. If you go again to the top right of the screen and click on the gear it will show you another drop down menu. In this menu click on the Add Disqus to Site button.

<img src="{{ root_url }}/images/homemenu.png"/>

## Step 3
Fill out the form like so:
<img src="{{ root_url }}/images/siteprofile.png"/>

Make sure that when you choose your Unique Disqus URL you are sure of it. So far Disqus has not given user's the option of changing it.

## Step 4
At this point in the game you can actually start working with your blog's source code. If you're up to date with Octopress you should already have the call to the Disqus partial inside your <span style="font-weight: 500">_layouts/post.html</span> partial.

The only thing you will need to change is inside the _includes/disqus.html partial. On line 4 you will need to put your unique short name for the site. If you can't remember it go to the admin page of your Disqus account, choose the unique site, and select the <span style="font-weight: 500">Settings</span> tab.

{% codeblock [disqus.html] %}
<script type="text/javascript">
    /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
    var disqus_shortname = 'YOUR_SHORTNAME_HERE'; // required: replace example with your forum shortname

    /* * * DON'T EDIT BELOW THIS LINE * * */
    (function() {
        var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
        dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
        (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
    })();
</script>
{% endcodeblock %}

And that's it! When you deploy your Octopress blog there will now be a comment section at the end of your blog posts.

