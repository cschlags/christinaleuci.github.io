---
layout: post
title: "Add Arrays To Your Rails Database"
date: 2014-12-22 13:38:32 -0500
comments: true
categories: [technical, rails, data structure]
---

Have you ever been looking at your database wondering how you can change that string or integer into an array or a hash?

<!--more-->

Before we start with the code make sure you have a database and it's model created in your Rails app.

I currently have it setup so that my Wardrobe is initiaized upon a User's creation. For a usual serialization you will only need two lines of code and optionally a method.

{% codeblock [wardrobe.rb] %}
  class Wardrobe < ActiveRecord::Base
    belongs_to :user
    serialize :wardrobe
    after_create :serialize_wardrobe

    def serialize_wardrobe
      self.wardrobe = {:tops => [], :bottoms => []}
      self.save!
    end
  end
{% endcodeblock %}

I used the serialize method that you can research more about <a href="http://apidock.com/rails/ActiveRecord/Base/serialize/class">here</a>. As a note although my class's name is Wardrobe I also have a param underneath that named wardrobe. You should use the param name you want to be serialized.

After serializing the wardrobe, which happens upon initialization, I decided to specify what would be inside that serialized wardrobe param by using an <span style="font-style: italics">after_create</span> method. Inside my after_create method I described that the wardrobe param hash should contain two keys with their respective arrays and it should be saved.

That's it! Keep on being badass programmers!