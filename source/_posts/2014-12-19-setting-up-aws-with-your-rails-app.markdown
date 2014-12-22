---
layout: post
title: "Using AWS Image Hosting for Rails"
date: 2014-12-19 12:17:11 -0500
comments: true
categories: [Technical, Rails, AWS]
---

If you're using only a couple of photos with your Rails application then it's pretty simple. If you're going to be using hundreds of thousands of photos you'll need a hosting site like Amazon S3.

<!--more-->
##Step 1 -- Have An Account

If you don't have an account with Amazon's S3 hosting service this step is for you. If you already have an account already please go to <span style="font-style: italics"> Step 2 </a>. Fortunately <a href="http://aws.amazon.com/s3/">Amazon</a>'s hosting services are free for the first 12-months. You will need to sign up for an account with your Amazon email and password. And that's it you're finished! You can now start integrating your Amazon S3 console with buckets and files. <span style="font-style: italics">A word to the wise: if you're new to Amazon S3 you should manually create a bucket inside your Amazon S3 account before accessing the AWS Client!</span>

##Step 3 -- Create A Pretty Access Key

As with any API you will need an account access key and an account password. You will be able to access your Security Credentials by going to the nav bar on the top and clicking either on your name or "My Account" depending on what page you're on and selecting Security Credentials. Click "Create New Key Access", this will show both your new access key and secret. You should download the Key and Secret file since your secret will no longer be available after you close the pop-up.

Awesome, now you have your access key and secret! Almost done!

##Step 2 -- Client Interface Dawg

After much trial and error I found the best approach to accessing Amazon S3 was to download the <a href="http://docs.aws.amazon.com/cli/latest/userguide/installing.html">AWS Client Line Interface</a> and the add a Bucket Policy Editor to the bucket's properties. The AWS CLI has two installations options which covers Windows, Unix, and Unix clone systems and it's pretty straight forward. Once that is installed you will need to type "aws configure" in your terminal while you are inside your application's main folder. I'm using Mac OSx so it may be alittle different for Windows based systems. This will prompt you for your access key, secret, region, and output format. I have found that the region and output format are not all too important for simple calls to the hosting site.

##Step 3 -- Super Cool Policy Creator

At this point your application now recognizes that AWS key and secret with it's config file. What you will now need to do is change the S3's bucket policy. By going into your Amazon S3 console and clicking on your bucket, the right side options <span style="font-style: italics">Properties</span> should light up. Click on this and click again on <span style="font-style: italics">Permissions</span>. Here you can see who has access to this bucket, if you haven't changed anything you should see only your username with all the following radio boxes checked.

Right underneath your name is a button called <span style="font-style:italics">Edit Bucket Policy</span>, click that. Here you can describe what actions people accessing your bucket will be able to perform. You can choose to use the <span style="font-style:italics">AWS Policy Generator</span> button but I have found that process to be convoluted and it ended up not giving me the policy I wanted.

Since this Bucket will only be used for the Curate Analytics site and no one else has access to the access key and secret I thought it best to allow all actions by placing a "*" for the AWS and Action lines. I may change this in the future but feel free to play around with actions. These Amazon S3 <a href="http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html">docs</a> are really great.

Don't forget to add YOUR bucketname down there!
{% codeblock [AWS Bucket Policy] %}
  {
    "Version": "2008-10-17",
    "Statement": [
      {
        "Sid": "Stmt1418573913031",
        "Effect": "Allow",
        "Principal": {
          "AWS": "*"
        },
        "Action": "s3:*",
        "Resource": "arn:aws:s3:::<bucket name>/*"
      }
    ]
  }
{% endcodeblock %}

##Step 4 -- All About That Code

You can finally start adding code to your Rails app. Inside the models you want the bucket to be accessed through you can write this line of code

{% codeblock [AWS Access Code] %}
  s3 = AWS::S3.new
    bucket = s3.buckets['<bucket name>']
    bucket.objects.each do |obj|
      if obj =~ /swipe batches/i && obj =~ /jpg/i
        self.sort_objs(obj.key)
      end
    end
{% endcodeblock %}

Like with any class you've initialized a new connection to the Amazon S3. Since your AWS account keys and secrets are hooked to this application's config you can call your S3 bucket.

Here I've gone through each obj which will start with the top folders and travel down to the actual file. An example of some json feed would be <span style="font-style: italics">tops, tops/collars, tops/collar/image.jpg</span> It's a big of a hassle but you should be able to use regex like I used above to find all files with a certai word in it. The objects in the json feed are a little odd but this works for my purposes, you'll need to play around with loops and functions to see what works for your bucket.

And you should have access to your S3 bucket and images inside it.

Keep on being badass programmers!