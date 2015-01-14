---
layout: post
title: "Arrays of a Feather Flock Together"
date: 2014-06-13 05:21:18 -0400
comments: true
categories: [Data Structures, Technical]
---

I have found that explaining programs (or trying to) has helped me greatly in understanding the use of syntax. So let me teach you about these little nuisances called Nested Arrays.

<!--more-->

Nested arrays were a big problem for me even in Java. I understood the concept that there was an array inside an array but the syntax and use never really connected. The NYC Pigeon Lab was pretty difficult for me. Taking one nested array and translating it into another was like climbing Mount Everest in under a minute.

The instructions for the NYC Pigeon Lab were define a method called nyc_pigeon_organizer and take in an array called pigeon_data. Take the names of the pigeon's first, a general attribute and the attribute specific to them. Easy in theory, hard in practice. 

{% codeblock [nyc_pigeon_organizer.rb] %}
{
  :color => {
    :purple => ["Theo", "Peter Jr.", "Lucky"],
    :grey => ["Theo", "Peter Jr.", "Ms .K"],
    :white => ["Queenie", "Andrew", "Ms .K", "Alex"],
    :brown => ["Queenie", "Alex"]
  },
  :gender => {
    :male => ["Alex", "Theo", "Peter Jr.", "Andrew", "Lucky"],
    :female => ["Queenie", "Ms .K"]
  },
  :lives{
    "Subway" => ["Theo", "Queenie"],
    "Central Park" => ["Alex", "Ms .K", "Lucky"],
    "Library" => ["Peter Jr."],
    "City Hall" => ["Andrew"]
  }
}
{% endcodeblock %}

Pigeon Data is a nested array so to get to the names you need to go through :color, then through :purple, then take each index in the name_array and put that into another array while deleting any repeats. But wait? Doesn't gender only have one list of names? If I put those names into an array then I wouldn't have to delete any repeats.

Well I tried that. Yes, I did get an array of names without complications and there were no repetitions but where could I go from there? I was completely lost. I had gotten into the gender hash and completely by-passed all that other information. What could I do now?

{% codeblock [nyc_pigeon_organizer.rb] %}
def nyc_pigeon_organizer(pigeon_data)
  pigeon_list={}

  :gender.each do |gender, name_array|
    pigeon_list[name_array] ||= {}
  end
  pigeon_list
end
{% endcodeblock %}

So I ultimately scrapped that and started out with a new method. This time I decided to take the long, multi-step path. I retrospect going through the pigeon_data array as a whole helped me learn more than just trying to go through the :gender hash.

So my new method went through the entire pigeon_data array, first the attributes like :color and :gender, then through their descriptive attirbutes like :purple and :male. Finally I was at the names but it was obvious I would have somewhere around five of each name in my new organized_pigeon array.

{% codeblock [nyc_pigeon_organizer.rb]%}
def nyc_pigeon_organizer(pigeon_data)
  pigeon_list = {}

  pigeon_data.each do |attribute, att_description|
    att_description.each do |attribute_specific, name_array|
      name_array.each do |name|

      end
    end
  end
  pigeon_list
end
{% endcodeblock %}

My first thought was to go through everything in an if loop and figure out if that name was already in the array or if it wasn't that I could add it. But I remembered, isn't there a way to see if there's already a hash in an array just like how you can see if the same string is in an array?

There is

{% codeblock [nyc_pigeon_organizer.rb]%}
  if pigeon_list[name]
    if pigeon_list[name][attribute]
      pigeon_list[name][attribute] << attribute_specific.to_s
    else attribute == :color
      pigeon_list[name][attribute] = [attribute_specific.to_s]
    else
      pigeon_list[name][attribute] = attribute_specific.to_s
    end
  end
else
  pigeon_list[name] = Hash.new
  if attribute == :color
    pigeon_list[name][attribute] = [attribute_specific.to_s]
  else
    pigeon_list[name][attribute] = attribute_specific.to_s
  end
end
{%endcodeblock%}

Let's go through the top parts of these if methods. In the first one I'm asking that if the pigeon_list[name] is already in use if it's not I create a new hasg through the use of Hash.new. If the name is new then I begin to add the attribute hashes that are connected to that name. I specified attribute == :color because that was a larger nested array than :gender or :lives. So whatever the attribute may be the attribute_specific (ie. the :purple, :male, or "Subway") is being changed into a string and added to that name-attribute array.

Very similarly the first if method is checking to see if that name is already a hash. If it is then it goes through the same process as the else that created a new hash. 
This is the complete code:

{%codeblock [nyc_pigeon_organizer.rb]%}
def nyc_pigeon_organizer(pigeon_data)
  pigeon_list = {}

  pigeon_data.each do |attribute, att_description|
    att_description.each do |attribute_specific, name_array|
      name_array.each do |name|
        if pigeon_list[name]
          if pigeon_list[name][attribute]
            pigeon_list[name][attribute] << attribute_specific.to_s
          else
            if attribute == :color
              pigeon_list[name][attribute] = [attribute_specific.to_s]
            else
              pigeon_list[name][attribute] = attribute_specific.to_s
            end
          end
        else
          pigeon_list[name] = Hash.new
          if attribute == :color
            pigeon_list[name][attribute] = [attribute_specific.to_s]
          else
            pigeon_list[name][attribute] = attribute_specific.to_s
          end
        end
      end
    end
  end
  pigeon_list
end
{%endcodeblock%}

When I first checked out this lab and read through it I thought it was impossible. In retrospect is was moderately complicated but easier now that I've gone through it and explained it to myself.

Keep on being badass!