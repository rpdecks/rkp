---
title: "How I Cut My Teeth Seeding My App from API’s"
date: 2020-05-28T11:07:10+06:00
author: Robert Phillips
image : "images/blog/blog-post-3.jpg"
bg_image: "images/featue-bg.jpg"
categories: ["Code"]
tags: ["API","Code", "Ruby"]
description: "not as intimidating as it seems from the outside looking in"
draft: false
type: "post"
---


## In this post I will:
1. Briefly explain what an API is
2. Share my experience growing learning to use API’s
3. Give a practical example using the Unsplash API

## What is an API?
> “API stands for Application Programming Interface. In layman’s terms, these interfaces are what allow software solutions to communicate with each other. It helps to think of them as the “engine under the hood,” and the backbone of the connectivity that our society has come to rely upon.”  
> [https://www.shippingschool.com/what-is-an-api-in-laymans-terms/](https://www.shippingschool.com/what-is-an-api-in-laymans-terms/)  

These two videos did a great job explaining an API in simple terms.  
**What is an API?**  
[![waiter image](http://img.youtube.com/vi/s7wmiS2mSXY/0.jpg)](http://www.youtube.com/watch?v=s7wmiS2mSXY)


**What Are APIs? — Simply Explained**  
[![API image](http://img.youtube.com/vi/OVvTv9Hy91Q/0.jpg)](http://www.youtube.com/watch?v=OVvTv9Hy91Q)

I appreciate the comparison of the API to the waiter in a restaurant. On one end there is a kitchen handling and processing raw elements and on the other end, a consumer… well, consuming. The waiter (i.e. API) handles the interface between the two so that the consumer can get, meaningfully, what he/she wanted from the kitchen, without all the messy behind the curtain detail, procedure, and process. Watch the videos, they do much better than me explaining… and the pictures help a lot too.  

For me, I am building a web application that has a “front” and “back” end to it. You could say the back is like the kitchen is with my database where I store everything and where so much of my system logic resides. You don’t need to get “under the hood” and know how it does what it does… it just needs to make you a delicious breakfast right? I used Ruby to code and build a rails system that serves all its data to the other side of my app, the front end. That’s where all that raw data is displayed and utilized so you and I can interact with it meaningfully. The front end is built using Javascript programming language with HTML/CSS to render and style it.  

## My experience
When I started learning, I was first acquainted with Faker, a Ruby gem, that is very useful for seeding your project with data to get you started. Both Faker and seeding are topics for another discussion, but in short, when you are building an app you need to test it as you build it. For this, it’s helps to have sample data in your app’s database so you can see what your app does with it. These tools are great for that.  

Eventually however, we all want our apps to help us meaningfully interact with real data and systems. Maybe that would be booking flights, weather forecasting, or shopping online. For me, growth meant wading into that water. I was intimidated at first but like everything so far, I find the bark is worse than the bite. If you’re curious and willing to take the time to read and tinker, you discover amazing, powerful things.  

I knew I needed to take it up a notch so I did with this project and was amazed with what’s out there free for our use. Almost anything you want to know about is being served and if you know how to interface or… API, then you can have it in your hands and in your app doing awesome stuff. For you it may be economic data, epidemiological info, topics, pictures, resources, etc. For me, this time, it was fish, lakes, and pictures.  

## Fish
**[Fishbaseapi API](https://fishbaseapi.readme.io/])**  
This resource had over 34,000 different kinds of fish in it with all kinds of details about them... a treasure to many from marine biologist to fisherman to aquarium hobby enthusiast. 
With a little bit of time reading the documentation and tinkering on their webpage interface I was able to see what kinds of data I could get back from a query and learn how to formulate a string query to ask their system for data.  
Google string query API. Getting familiar with the data and the queries proved helpful when I got to the next tool I needed.  

**Rest-Client**
This is a Ruby gem you can install/require in your rails project that will query an API. Here is a simple snippet of what I did in my seed file with it.  
```
  # FISH API
  [FISH_url = ‘https://fishbase.ropensci.org/species'](‘https://fishbase.ropensci.org/species)
  res = RestClient.get FISH_url, params: {limit: 50}
  # isolates key with fish data
  res_ary = JSON.parse(res)[“data”]
  # filters the array to get better curated collection of fish for our app
  fish_ary = res_ary.select { |fish| fish[“FBname”] != nil && fish[“GameFish”] = -1 && fish[“Fresh”] = -1 }
  # create seed data
  fish_ary.each do |fish|
    Fish.create(species: fish[“FBname”], description: fish[“Comments”], image: fish[“image”], is_active: true)
  end
```  

## Lakes
**[State of NY API powered by Socrata](https://dev.socrata.com/foundry/data.ny.gov/f4vj-p8y5)**  
I discovered that the New York makes all kinds of useful data it has available to the public for use and consumption. I love it. I suspect having a resource like this really empowers developers to build resources that should enhance New Yorkers’ quality of life, operational efficiency, etc.  
Anyhow, there are lakes and rivers all over NY. The API endpoint for them had lots of detail about each. I had to get familiar with another tool:  
Socrata.com… “the future of connected government” as they say.
Great and powerful with data. You just need to get set up with a developer account and get access keys for your app. With those keys, you can install/require the “soda-ruby” gem (see Github) and start to make your first GET request to the state of NY API endpoint. I think of API endpoints being like the place where a pipe for this kind of data sticks out of the internet for you to connect to. Anyhow, search for API endpoints here https://dev.socrata.com/data/.  
Here’s something simple I used in my seed file to get some actual lakes and rivers in NY into my app.
```
  # NY State Spot API
  SPOT_url = “https://data.ny.gov/resource/f4vj-p8y5.json"
  # have to set up account to get app token and thereby permission to ping/request
  # instantiates new request
  client = SODA::Client.new({:domain => “data.ny.gov”, :app_token => “INSERT YOURS HERE”})
  # JSON GET request
  response = client.get(“f4vj-p8y5”, :$limit => 20)
  spots = response.body
```  

## 3. Pictures
**[ Unsplash.com ](https://www.unsplash.com)**  
I love Unsplash. Tons of pictures available in the free public domain (so long as you take care for their guidelines for use). You can find pictures and collections thereof of just about anything. I wanted lakes and rivers for my app. Here are some steps I stumbled through below to get my app seeded with amazing pictures.  

---

### Unsplash example:  

**1. Read the docs:** => [https://unsplash.com/documentation#getting-started](https://unsplash.com/documentation#getting-started)  
It’s all there and does an OK job explaining how get started. I am newer to this but I’m in school for it and so I could make my way around and understand the jargon. It may be harder for you if you’re new. If you’re a pro… then please go easy on me in the comments :) Amazing you’re still reading…  
Anyhow, what really helped me after familiarizing with the docs was to:

**2. Watch this video:**
Using the Unsplash API — Tutorial
<a href="http://www.youtube.com/watch?feature=player_embedded&v=95wNOAoSyaQ
" target="_blank"><img src="http://img.youtube.com/vi/95wNOAoSyaQ/0.jpg" 
alt="unsplash logo" width="240" height="180" border="10" /></a>
I could not use my browser to play with the data like I could with the fish above. You have to set up an account as a developer again and get a key. But this time, set up was different. The video shows you how to use software called Postman. Postman lets you act like a browser and make requests to servers and API’s like this. Using my key, I could request , get a response, and see what it looked like.

**3. Seed file**  
From there I could get into my seeds file and configure it to request the data rightly and use the response to seed my data appropriately. Again, you will need a gem. Install the “unsplash” gem in Ruby and require it in your seed file. Here’s the repo for that gem with it’s documentation [https://github.com/unsplash/unsplash_rb](https://github.com/unsplash/unsplash_rb)  

**4. Read the Unsplash gem documentation**  
Find this link in the README for more of the gory detail about how the client works to improve the quality of your GET request syntax. (http://www.rubydoc.info/github/unsplash/unsplash_rb/)
Here is the snippet from my file:
```
# FISHING SPOT SEEDING
# Unsplash spot image API
lake_res = Unsplash::Photo.search(‘lakes’, page = 1, per_page = 30)
```  

Finally, use <a href="/blog-post">binding.pry</a>. I have another blog about that. It’s a great debugging tool. I would run rake db:seed with a pry inside the seed file and check to see what data I got back when the program ran into the pry. With it paused there, I could then handle the array or hash of data and improve the formatting of my request or syntax and storage of the data so that ultimately… I had gorgeous pictures in my app.  

One day, my users will upload their own. But for now, it’s pretty and paints a picture. Now, if the product would only be viable. At least I could propose something to a client with actual relevant pictures instead of Faker avatars for lakes. Even if not that, I learned a lot and I grew. I think that was the point.
I hope this helps you. It was a great learning experience for me. On to the next.


Photo by [Tudor Baciu](https://unsplash.com/@baciutudor?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/coffee-code-computer?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText)