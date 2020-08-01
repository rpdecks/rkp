---
title: "VIM editor - A carpenter and his tools…"
date: 2020-05-07T11:07:10+06:00
author: Robert Phillips
image : "images/blog/tools.jpg"
bg_image: "images/featue-bg.jpg"
categories: ["Code","Vim"]
tags: ["Advice","Code"]
description: "my experience learning to use the VIM editor"
draft: false
type: "post"
---


They say a "carpenter is only as good as his tools. I am not sure I swallow that in full but I know many a carpenter who enjoys his tools as much or more than the any of the things built with those tools. I think I understand. They live and work with them every day and they live on with him from job to job. You may say they are a big part of his quality of life.  

In my journey to become a software engineer, I enrolled in a coding bootcamp and began to cut my teeth. I am finding this is all truly "grist to my mill." I really love waking up each day to a whole new world, challenges, learning, and growth. The goal for my cohort is to become functional web development professionals this year. So for this carpenter, my finished work now resembles a living breathing web application like so many others we have become so familiar with… a news app, Twitter, Instagram, Amazon, online banking, shopping, etc…  

As cool as those are, I have become quite excited with tools of the trade and learning to use them. I am finding as much joy in them as the product they make. For carpenters, it may be laser levels, state-of-the-art saws, nail guns, nifty clamps, etc. For programmers, it is things like keyboards, editors, plugins… plugins for sure. In both cases, these are things that make the job easier and the product more refined, consistent, accurate…  

## Enter VIM
VIM is an uber powerful text editor. Text editors are to programmers as drills are carpenters. They work with wood, we work with text. As they will tell you, and as I am learning… both are an art. I used to think of text stuff found in books about which I had to write and turn in papers. That was something done using a word processor and a printer. Now it is becoming paint to me and VIM, the most amazing of brushes…  

**What is VIM?**  
Vim is a modal text editor. This means it has various modes of operation, the most basic of which took a revolution of concept for me. There are 6 basic modes of use (there are others but they are variations of these). The name of the game is efficiency and functionality. It is compact yet highly customizable. It's over 40 years old yet remains among the sharpest and most powerful of tools in this industry. It's highly versatile across all the major platforms.  

It's not designed for presentation like other word processor or design programs we commonly think of, but I'm using it to write this. In this post, I will mainly detail the first and most primary of its 6 basic modes of operation.  

**Normal mode:**  
Normal mode was the most abnormal for me at first. I needed a big change of concept.  

Most of us would consider a normal mode of operating with text to be one in which our pen is to paper or "brush is to the canvas" as Drew Neil puts it in his book "Practical Vim." We would expect normal to be fingers on keys and letters appearing on a screen. Normal mode however is much different in Vim.  

Normal is a mode where you have the text in front of you, on the ground level, and yet you are a level above that in abstraction. When you put fingers on keys, you first start to move around the canvas and, as you choose, to manipulate and maneuver text you contact somewhat like puzzle pieces on your dining table. At times, you need to type text, for sure, but I am finding that MUCH of our time programming involves moving things around like pages on a desk or tiles checkers on a board or repeating actions on bodies of text. We are trying pieces here, then switching things up and trying them again. We are massaging a workpiece until things fit just right (and our tests pass!!).  

I would liken this to a block mason or tile setter building a wall out of custom stone. He starts with a pile of rocks and all day long shapes them and the wall, moving, trimming, and fitting them together into a formation that becomes a beautiful fireplace or shower.  

This is where Vim amazes me and where I am learning to think about and handle text in an entirely new way. Vim has many tools that let you do these things with text in many different ways. I am daily discovering new ways and features of this tool that makes working with text so fun. It's like a daily treasure hunt with discovery. Here are few I am enjoying so far:  

**=> copy/pasting… yanking/putting**  

I used to see text as something that had to be highlighted with a mouse, right-clicked, cut/copies, then pasted somewhere else. It's great. Such a time saver and really helps my spelling :). With Vim I have found do many ways to do this.  

With a few keystrokes I can:
1. yank a whole word, a whole line, or everything up to that apostrophe, etc and hold it in "register spot a'
2. then I can yank everything from my cursor to the end of the file and save that into "register spot b"
3. then I can maneuver into another file and, in a few more keystrokes, paste a here, then b there, then b again, then a at the end. Nice.
4. oops! wrong spot? "u" (undo), "kk" (up up), "p" (put) it there instead. Really nice :)

I found out I could change a word SO easily in many ways:  
1. I put my cursor on the word
2. hit 3 keys and it all disappears
3. now I can type the word (insert mode) I want there instead. Nice.
4. THEN… it gets powerful. I can repeat that with a single "." many times over here, there, and everywhere.

When coding, we often have something like the follows
```
  def create
  @store = Store.new(store_params)
  @store.user_id = current_user.id
  if current_user.role == 2
      if @store.valid?
      @store.save
```
Very often, we want to change or reuse this code elsewhere. What if store needs to be "item." This made me so happy the day I learned how to do this.
1. I can do 'ciw' (change inner word) to "store", type "item", and then hit 'escape' to switch out of insert mode back into normal mode.
2. Why? To navigate around using fun keys I mapped for up down etc. I can skip over words with others. Then, in fun normal mode, I land on "store" again (anywhere in the word!), and I hit that glorious little period "."
3. What happens? You guesssed it. Changed! "item" it is. Dot repeat. Love it!

This is just a little example. I discovered this over the last two weeks. It's changed my daily life in a small way but one that makes me happy and is fun to me.  

Every day I have been climbing a learning curve. Everyone around me is so fast and good using other tools… but I have to say. This is fun. Every day I am discovering things about my tools that makes this so fun. Not only that, my whole way of thinking about text and work are changing.  

Maybe in later posts I will say more about these other modes below. If this has whet your appetite, look up iTerm2. Get it. Open it. Go to the command line, and type vimtutor. It will walk you through the basics and have you moving around in Vim in an hour or so. It will feel funny at first, but it's worth it to ride this bike.  

**Visual mode:**  
Similar to normal mode but you can highlight regions of text and us normal mode operations on the selection.  

**Select mode:**  
I am least familiar with this one so far but it's a lot like visual mode. It allows you to use arrows to extend a selection more like what we expect shift+arrow key to do.  

**Insert mode:**  
This is the mode we are all used to when we open a word processor like Google Docs, MS Word, etc.  

**Command-line mode:**  
This is like a terminal mode inside vim you can use to do things like changes settings, views, search, and use some shell commands.  

**Ex mode:**  
This is like a command-line mode just mentioned but it remains open after entering a command until you exit the mode.


<span>Photo by <a href="https://unsplash.com/@pjswinburn?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Philip Swinburn</a> on <a href="https://unsplash.com/s/photos/woodworking?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></span>