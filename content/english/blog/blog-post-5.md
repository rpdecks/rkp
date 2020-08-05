---
title: "Static Site Generators Pros, Cons, & Suprises"
date: 2020-08-04T08:58:52-04:00
author: Robert Phillips
image : "images/blog/static.jpg"
bg_image: "images/blog/static.jpg"
categories: ["Code"]
tags: ["Hugo","SSG"]
description: "pro's and con's of static site generators"
draft: false
type: "post"
---

## What are 'Static Site Generators' (SSG's), what are their pro's and cons, and when should we use them?

When building my own personal portfolio website, I chose to lean into static site generators and learn something new. The personal portfolio is an excellent use case for a Hugo site hosted somewhere like Netlify, and that is just what I did with [robertkevinphillips.com](https://robertkevinphillips.com).

### In this post I will:
1. Explain what SSG's are
1. Cover pro's & cons
1. List some popular SSG sites that may suprise you

## What are Static Site Generators (SSG's)?
The name here tells us a lot. The implication, obvious, that some sites are static while others are dynamic. In the case of static, think fixed or constant. This means that, just like creating a Word document and clicking save, developers create HTML content for a whole website (perhaps many pages) and save that version of the content. All of this content is then uploaded to a server where it can be delivered upon request, quite well by the way. More on that later.

Dynamic sites are more complex. As the name implies, these kinds of sites are changing, perhaps constantly. Think stock ticker, news, social media sites, etc. New tweets are published, then liked, then retweeted, etc. These kinds of sites are typically powered by a content management system (CMS) of some sort. These are sites like WordPress, Drupal, Joomla. This means that these websites are quite literally built upon request time and again for each request. On top of that, while you are viewing it, it is often changing dynamically as you scroll. For a WP site, it could just be a plugin with a widget that pull in/displays your company Google calendar. Dynamic sites usually have some sort of front end interface and back end database that lives on a server. When a user visits the site URL, data is fetched from the database and various other sources on the web and your view gets built for you and delivered to your browser.

Static site generators essentially take the static content (files) described in the first paragraph and build them into websites. This means files and folders are put onto a server and delivered upon request. It also means there is no database with complex server side processing to query the database and construct content for the user. Constructed content exists already and is merely served upon request from existing files and folders.

## Advantages of the SSG
### **Speed**  
- **53% of retail consumers will abadondon a website if it takes longer than 3 seconds to load** ([Research](https://www.marketingdive.com/news/google-53-of-mobile-users-abandon-sites-that-take-over-3-seconds-to-load/426070/)).  
- SSG's already have all the content locked, loaded, and ready to fire upon request while CMS type sites must run network requests to fetch data from API's (think pull breaking news stories from Reuters, etc), query the database to pull up records, and construct the pages you are to be served once you click go.  
- **How?** The work of building is shifted away from time of request. This means the server only has to find and serve a file, perhaps in mass. There is no way to beat this in terms of speed.
- **CDN's** - Content delivery networks drastically optimize performance and user experience. How? They are geographically distributed group of servers which work together to provide fast delivery of Internet content. This means, the content is cached on servers around the globe and served to you from the closest one when you request it.

### **Scalable**   
- **CDN's again** - when you can store cached versions of your site all around the globe and respond ultra fast to requests, you are poised to scale up fast.
- **Simplified hosting** - Serverless in the sense of removing databases and layers of server side logic built around controlling/routing/building that content
- **Cached content** - easier to scale than integreted logic communicating front to back etc.

### **Relability**  
- **less moving parts** means less things to break. [Think CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) 

### **Security** . 
- Since SSG's are rendered in advance, this leaves much fewer avenues for cyber attack. It is when we have data flowing from a front end to a back end database we are more exposed to intercepts and attacks from things like database injections that lead to corruption, breaches, and security compromises.
- This is a big drawback for WordPress type CMS sites. These sites have thousands of plugins people use to do things like display the the company calendar, recent blog posts, reviews, etc. on their site. Thousands of plugins... some better than others... these are often the subject of malicious attacks.
- WordPress type sites have admin panels that allow users to manage content and edit/maintain their website. Again, these get logins get hacked by bad actors.
- "There is no server more secure than the one that does not exist."

### **Efficiency**  
- Content delivery & speed improve efficient all around (servers, users, etc)
- Ease of maintenance (more on this later) means efficiency boost
- Inexpensive in terms of cost & resources

### **Ease of maintenance**  
- **Host site anywhere:** can essentially be hosted in a Dropbox folder, AWS bucket, Github repo, etc)
- **Version control:** How nice to be able to rollback to any version of the site in GitHub commit history with a few keystrokes? Easy to fix errors. Not so easy with CMS. Databases can be irreversibly corrupted (ie. db DROP TABLE). This equals built-in site backup.
- Less time patching for security
- Easier to debug (think front end back end bug tracing gone)
- Faster, easier updates (think git push heroku master, wait, then db:migrate, then db:seed, etc...)
- Edit content offline and push later

### **Pick your langauge**   
- Available utilizing many languagues
    * Ruby? (Jekyll, Slate...)
    * Javascript? (Next.js, Gatsby...) 
    * Go? (Hugo)
    * Pyton? (Pelican, MkDocs...)
    * etc... see [StaticGen](https://www.staticgen.com/)
### Flexibility  
- Enter [JAMstack](https://jamstack.org/): 
    * Is a modern web development architecture based on client-side JavaScript, APIs and Markup (JAM)
    * Add JS to your site, fetch data from other API's real time and get some of the benefits of a dynamic CMS without the backend db (ie pull weather data, etc to effectively build your own widgets).

## Drawbacks of the SSG
1. Cannot handle highly complex systems that need to be dynamic (i.e. they live and breathe based on real, evolving data like Twitter, FB, Instagram etc.) If your site needs to have forms, create profiles, involve commenting, messaging, reviews, etc. These kinds of sites really need a more traditional dynamic configuration.
1. No user interface for site management: WordPress for example has an admin panel where less technical users can make changes to the website without knowing how to code, write in markdown, etc.
1. WordPress market share: there MANY more WP themes out there, lots of online support from a large user base, and WP the most likely to be around for the long haul in some form.
1. Can grow stale faster: Dynamic sites keep living and breathing while static sites need new content being pushed. That said, many kinds of sites are informational and non-changing and thus remain more relevant and fresh given that nature.

## Prominent names built on [Hugo](https://gohugo.io/)
1. [Netlify](https://netlify.com)
1. [1Password password manager](https://1password.com/)
1. [Bed Bath Beyond video subdomain](https://video.bedbathandbeyond.com/)
1. [Advance Auto Parts video subdomain](https://video.advanceautoparts.com/)

## Prominent names built on [Next.js](https://nextjs.org/showcase)
1. [Nike](https://devices.netflix.com/en/)
1. [Staples](https://m.staples.com)
1. [Hulu](https://hulu.com)
1. [Twitch](https://m.twitch.tv)
1. [TikTok](https://tiktok.com/en/)

## Prominent names built on [Jekyll](https://jekyllrb.com/showcase/)
1. [Netflix Devices](https://devices.netflix.com/en/)
1. [Spotify Developer page](https://developer.spotify.com/)
1. [Bitcoin](https://bitcoin.org/en/)
1. [Ruby on Rails](https://rubyonrails.org/)

Photo by [George Coletrain](https://unsplash.com/@georgecoletrain?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/static?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText)