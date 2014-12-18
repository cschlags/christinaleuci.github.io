---
layout: post
title: "Setting Up An API On An Existing Rails Application"
date: 2014-12-18 12:48:34 -0500
comments: true
categories: [Technical, Ruby, API]
---

Hey y'all it's been a long time! Life has been a bit busy at the moment and I'll write a blog all about that in the non-technical session ASAP.

Today I'm going to talk about setting up your current Rails application with an API. This is assuming that you have a working Rails application, preferably with data to see in the browser when you have it up and running through the rails server.
<!--more-->

##The Pre-Coding Code
So before we do any serious setup the first thing we will need to do is access the rails-api gem. To do this put this line inside your Gemfile file 

{% codeblock [Gemfile] %}
  gem 'rails-api' , require: 'rails-api/action_controller/api'
{% endcodeblock %}

and bundle install.

##Ain't No Basic Controller
At this point you will have access to the API Action Controller which I suggest you read up on in the <a href="http://api.rubyonrails.org/">Ruby on Rails API docs</a>. So now that your application is ready for API goodness you'll want to create a sub-folder underneath your <span font-style="italics">app/controllers</span> titled <span style="font-weight: 500"> api </a>.

At this point underneath <span font-style="italics">app/controllers</span> you should have your main controllers, ie. application_controller or user_controller, and your <span font-style="italics">api</span> folder.

Now inside this api folder you will want to add an <span font-style="italics">api_controller.rb</span> file and an optional <span font-style="italics">v1</span> folder.

The <span font-style="italics">api_controller.rb</span> will be your API controller, this is essentially the application controller for your API. Inside your api_controller you will want to set it up similarly to the application_controller {% codeblock [api_controller.rb] %}
  class Api::ApiController < ActionController::API
  end
{% endcodeblock %}

As you can see we are no longer using the Base Action Controller, instead we're using the API Action Controller. This will be one of the sub controllers that will inherit from the Base Action Controller. If in the future you want to implement an API sign in option with a username and secret as many api's have now-a-days this if the file you would do this in.

At this point you should have the <span font-style="italics">api_controller.rb</span> setup and hopefully, but not required, the <span font-style="italics">v1</span> folder setup.

I suggest implement the <span font-style="italics">v1</span> folder incase you decide to create multiple versions of the api in the future.

So let's say you have the <span font-style="italics">v1</span> setup. Inside this folder you will have all of the controllers you're find in the main controllers folder. Currently I am using an api user_controller to allow api users to access the users in the system.

##Get The Data You Want
Inside the <span font-style="italics">user_controller.rb</span> you will want to set it up depending on which information you want to be shown. Let's say that your application user_controller has both index and show methods, this will mean your API user_controller will also have index and show methods.

{% codeblock [api/v1/user_controller.rb]%}
  class Api::V1::UserController < Api::ApiController
  include ActionController::MimeResponds
    def index
      respond_to do |format|
        @users = User.all
        format.json { render json:@users }
      end
    end
    def show
      respond_to do |format|
        @user = User.find(params[:id])
        format.json { render json:@user }
      end
    end
  end
{% endcodeblock %}

Here I have set it up so that when the api user goes to <span font-style="italics">example.com/api/v1/users.json</span> they will see all the users in the database. As you can see I put ".json" at the end of the html address, this is because inside the api user_controller I specified the format I want to respond to. I you wanted the api to respond to an xml call then you would put 

{% codeblock [user_controller.rb] %}
  format.xml {render xml:@user}
{% endcodeblock %}

 or even allowing access to both json and xml calls

 {% codeblock [user_controller.rb] %}
    if format.json {render json:@user}
    if format.xml {render xml:@user}
 {% endcodeblock %}

 I setup the user.show api call similarly. When an api user calls <span font-style="italics">example.com/api/v1/users/1.json</span> this will show that specific user's data.

##Almost Done
Awesome, you're almost done. Just like any other controller you will need to set up the routes. 
{% codeblock [config/routes.rb] %}
  namespace :api do
    namespace :v1 do
      resources :user do
        resources :other_controllers
      end
    end
  end
{% endcodeblock %}

I chose to organize my api groups of controllers under a namespace and again under a v1 namespace. This allows me to organize my routes instead of searching for them through the document. Optionally I chose to make my <span font-style="italics">api/user_controller.rb</span> also a namespace. This requires me to access my other api controllers based on the user api controller.

And that's it! You have successfully implemented your application with Rails API. Any advice or concerns are appreciated! <3
