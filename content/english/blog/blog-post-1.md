---
title: "Why I Love to Pry... Now"
date: 2020-04-15T11:07:10+06:00
author: Robert Phillips
image : "images/blog/hammer.jpg"
bg_image: "images/featue-bg.jpg"
categories: ["Code", "Ruby"]
tags: ["Advice","Technology"]
description: "this is meta description"
draft: false
type: "post"
---


---

Some say a carpenter is only as good as his tools. I think I understand a lot more now…  

I recently began my journey toward becoming a full-stack web developer with Flatiron School. I have been learning to program in Ruby and have come to treasure this debugging tool called "Pry." This tool has been a game-changer for me…  

Here is a breakdown of what Pry is and why you should use it.  

# What is Pry?
> Pry is an interactive shell for the Ruby programming language. It is notable for its Smalltalk-inspired ability to start a REPL within a running program. This lets programmers debug and modify the current state of a system.
> -[Pry (software) | Wikipedia](https://en.wikipedia.org/wiki/Pry_(software))  

When we write programs, we need a way to interact with that program and it's behavior such that we can understand how it is working and why things are happening on the inside.  

"REPL" means Read - Evaluate- Print - Loop and does just that. Ruby installs with it's own built in REPL named IRB (Interactive Ruby) which is very useful in this way. I use it daily and highly recommend it as well…  

Pry, however, is another of these tools that is a Ruby gem. This basically means it's a package of code that you can implement by installing and requiring it your Ruby application files(see how here).  

IRB is a sort of sandbox Ruby environment you can access at your command prompt wherein you can "play" with Ruby syntax, methods, variables, operations, etc. Pry is a tool that allows you to place a "wedge" inside your Ruby program that will interrupt that program and the place of your choosing. When it interrupts it, it also gives you the ability to step inside that program and see what's going on.  

![engine](https://unsplash.com/photos/xFjti9rYILo "Engine")
# Storytime
I come from a mechanical background. I have grown up working on cars, air conditioning systems, etc. Many times, when my car had a problem it made noises, smoked, smelled, vibrated, etc. These were all clues to me that something was wrong. What I always wished I could do was get inside the motor, or at least put a camera in there, so I could see what was rattling, clanging, etc. If I could do that, I felt I could have understood the problem and found the solution. In the absence of that, I often just started swapping out the parts until the problem went away. Sometimes I never understood what happened and I almost always replaced good parts while I guessed at a solution.  

Fast-forward, when I began to program I found myself doing the same thing. I would write Ruby code, run tests, hope it would pass, and then if it would fail, I would guess at something else. I spent so much time "poking and hoping" with no process and no camera inside…  

That is until Pry descended from above. It was what I had always longed for as a mechanic… and now as a programmer.  

# SOME things you can do with Pry
## Debug
No doubt, Pry gets you inside. Ruby error messages and Ruby tests designed to test our code for us will give us clues. Understanding error messages is another post for another day, but suffice it to say that in the error you will usually get a hint where to go put a Pry into your code.  

Go to that spot, and somewhere before the error occurs… you can type binding.pry… like this:  

## Dig
Now that I am using Pry, I find it's a great way to see what I have access to at any point in a program. Just like a prying nosey neighbor, don't be bashful. Get in there and be curious. I find this is especially useful when I am returning values and passing around arguments between methods.  

In Pry, I find myself typing "self" a lot. Doing so will help you to understand internal execution context. It's helpful to know what you are and where you are. For example, I'm a new instance of "Honda" that was just created. Inside Pry, I can ask questions like 'honda.class' and hit enter to see I'm a Car object. The way my program is set up, Cars have a variable set up for them named "value." I can enter honda.value and find out that value=nil. So now I know value is not set yet and then figure out what to do next.  

In Ruby, we deal with objects that have all kinds of relationships. Once it comes time to save user data in our databases and associate all of that data, this ability goes to another level. Pry is such a powerful tool. It lets you dig around inside your program and your data to see what is there. Often, you then find out what to do next and where to go. Then comes ActiveRecord… another post for another day :)

## Test run
I love the ability to put a pry in my code at a point where I am unsure what to do next. I am finding it so helpful to "get in there" with a pry and starting writing and test driving my next lines of code. It's super helpful, especially when you're just starting out. It helps me to lay down good code I know works because of a fast feedback loop…. REPL… read, evaluate, print… loop :)
```def self.display_all_drinks
  binding.pry
  Drink.all.each_with_index do |val, index|
    binding.pry
    puts "#{index + 1}. #{val.name} --> Drink ID (#{val.id})"
  end
end
```
In this real example, putting this pry in helped me to write this code correctly the first time. I started with the method defined. Then I ran the program and hit the pry. The pry let me step in and see what what going on. Typing Drink.all showed me I was dealing with an array of objects. I wanted to show them to my user. I wanted to see them as a list. A Google search for the Ruby enumerable lead me to each_with_index. Then I wanted to understand what "val" and "index" were and where those values were coming from. When I ran into the pry, I was able to type "val" and then "index" and see these were the object in the array along with its position (index) in the array. Armed with that. I tried to type out my puts statement… then it dawned on me, I could just paste it and see if it worked. I tried several versions of that until I got a puts output I liked… then I wrote that working code into my method.  

Before Pry, I would have written something I thought worked… then wrote more methods like that and tried to run it all. It would take forever to find the problem and I never got it right the first time… until I started doing this.

---

So now, I compose with Pry. That was a big breakthrough for me. A few lines of code, then I Pry. Does that work… what will I write next? Let me pry. You pry too… don't worry… it's not rude to Pry, not like this at least :)
I hope this helps you… especially if you're just getting started like me.



Hammer:
<span>Photo by <a href="https://unsplash.com/@imattsmart?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">iMattSmart</a> on <a href="https://unsplash.com/s/photos/engine?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></span>


Engine:
<span>Photo by <a href="https://unsplash.com/@garett3?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Garett Mizunaka</a> on <a href="https://unsplash.com/s/photos/engine?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></span>