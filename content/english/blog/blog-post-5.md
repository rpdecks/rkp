---
title: "Static Site Generators Pros, Cons, & Suprises"
date: 2020-08-04T08:58:52-04:00
author: Robert Phillips
image : "images/blog/static.jpg"
bg_image: "images/blog/static.jpg"
categories: ["Code"]
tags: ["Hugo","SSG"]
description: "pro's and con's of static site generators"
draft: false/
type: "post"
---

Photo by [George Coletrain](https://unsplash.com/@georgecoletrain?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/static?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText)
## What are 'Static Site Generators' (SSG's), what are their pros and cons, and when should we use them?

When building my own personal portfolio website, I chose to lean into static site generators and learn something new. The personal portfolio is an excellent use case for a Hugo site hosted somewhere like Netlify, and that is just what I did with my site <a href="robertkevinphillips.com" target="_blank">RobertKevinPhillips.com</a>

### In this post I will:
1. Explain what SSG's are
1. Cover pros & cons
1. List some popular SSG sites that may suprise you

## What are Static Site Generators (SSG's)?
The name here tells us a lot. The implication, obvious, that some sites are static while others are dynamic. In the case of static, think fixed or constant content. This means that, just like creating a Word document and clicking save, developers create HTML content for a whole website (perhaps many pages thereof) and save that version of the content. They then upload all of this content server where it can be delivered upon request to browsers, and quite well by the way. More on that later...

Dynamic sites are more complex. As the name implies, these kinds of sites are changing, perhaps constantly. Think stock ticker, news, social media sites, etc. New tweets are published, then liked, then retweeted, and so on. These kinds of sites are typically powered by a content management system (CMS) of some sort. These are sites like WordPress, Drupal, Joomla. This means that these websites are quite literally built at the time of user request over and over and again for each request. Additionally, while you are viewing it, it is often changing dynamically, as you scroll for example. For a WP site, that may just be a plugin with a widget that pulls in and displays your company's Google calendar. Dynamic sites usually have some sort of front end interface and back end database that live on a server. When a user visits the site URL, data is fetched from the database and perhaps other sources on the web while your view is built for you and delivered to your browser.

Static site generators essentially take static content (files, folders) and build them into websites. These files are put onto a server and delivered upon request. This is done without databases with complex server-side processing to query those databases to construct content for the user. The constructed content exists already and is quickly and simply transmitted to the user.

## Advantages of the SSG
### **Speed**  
- **53% of retail consumers will abadondon a website if it takes longer than 3 seconds to load** <a href='https://www.marketingdive.com/news/google-53-of-mobile-users-abandon-sites-that-take-over-3-seconds-to-load/426070/' target='_blank')Article</a>
- SSG's already have all the content locked, loaded, and ready to fire upon request while CMS type sites must run network requests to fetch data from API's. News sites need to scour the internet for new stories. The app needs to also query the database and construct content in aforementioned ways.  
- **How?** The work of building is shifted away from time of request. This means the server only has to find and serve a file, perhaps in mass. There is no way to beat this in terms of speed.
- **CDN's** - Content delivery networks drastically optimize performance and user experience. How? They are geographically distributed group of servers which work together to provide fast delivery of internet content. Content is cached on servers around the globe and served to users from the closest one when requested.

### **Scalable**   
- **CDN's again** - With cached versions of your site all around the globe, sites can scale up fast and handle giant volumes of traffic at once.
- **Simplified hosting** - Serverless in the sense of removing databases and layers of server side logic built around controlling/routing/building that content.
- **Cached content** - Makes it easier to scale up vs integreted CMS systems with dependent logic like apps with communicating front and back ends.

### **Relability**  
- **Less moving parts** means less things to break. <a href='https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS' target='_blank'>Think CORS</a>  

### **Security**  
- Since SSG's are rendered in advance, this leaves much less avenues for cyber attack. Data flowing from a front end to a back end database is much more exposed to intercept and attack from things like database injections that lead to breaches and compromises.
- This is a big drawback for WordPress type CMS sites. These sites have thousands of plugins people use to do things like display the the company calendar, recent blog posts, or reviews on their site. There are thousands of plugins... some better than others. These avenues are often attacked.
- WordPress type sites have admin panels that allow users to manage content and edit/maintain their website. Again, these get logins get hacked by bad actors.
- "There is no server more secure than the one that does not exist."

### **Efficiency**  
- Content delivery & speed improve efficient all around (servers, users, etc)
- Ease of maintenance boosts efficiency
- Inexpensive in terms of cost & resources

### **Ease of maintenance**  
- **Host site anywhere:** can essentially be hosted in a Dropbox folder, AWS S3 bucket, Github repo, etc.
- **Version control:** How nice to be able to rollback to any version of the site in the GitHub commit history with a few keystrokes? It's easy to fix content or commit errors. It's not so easy with CMS. Databases can be irreversibly corrupted (oops, db DROP TABLE??). This equals built-in site backup.
- Less time patching for security
- Easier to debug (think front end, back end bug tracing gone)
- Faster, easier updates (think git push heroku master, wait, then db:migrate, then db:seed, etc...)
- Edit content offline and push later

### **Pick your langauge**   
- Available utilizing many programming languagues
    * Ruby? (Jekyll, Slate...)
    * Javascript? (Next.js, Gatsby...) 
    * Go? (Hugo)
    * Pyton? (Pelican, MkDocs...)
    * etc... see <a href='www.staticgen.com' target='_blank'>StaticGen</a>

### **Flexibility**  
- Enter <a href='jamstack.org' target='_blank'>JAMstack</a>  
    * Modern web development architecture based on client-side JavaScript, APIs and Markup (JAM)
    * Add JS to your site and fetch data from other API's real time to get some of the benefits of a dynamic CMS without the backend db (example: pull weather data, etc to effectively build your own, safer widgets).

## Drawbacks of the SSG
1. Cannot handle highly complex systems that need to be dynamic (i.e. they live and breathe based on real, evolving data like Twitter, FB, Instagram etc.) If your site needs to have forms, create profiles, save records, involve commenting, messaging, reviews, etc. These kinds of sites really need a more traditional dynamic configuration with a database.
1. No user interface for site management: WordPress for example has an admin panel where less technical users can make changes to the website without knowing how to code, write in markdown, etc.
1. WordPress market share: there MANY more WP themes out there, lots of online support from a large user base, and WP the most likely to be around for the long haul in some form.
1. Can grow stale faster: Dynamic sites keep living and breathing while static sites need new content being pushed. That said, many kinds of sites are informational and non-changing and thus remain more relevant and fresh given that nature.

## Prominent names built on [Hugo](https://gohugo.io/)
1. <a href='https://netlify.com' target='_blank'>Netlify<a/>
1. <a href='https://1password.com/' target='_blank'>1Password password manager<a/>
1. <a href='https://video.bedbathandbeyond.com/' target='_blank'>Bed Bath Beyond video subdomain<a/>
1. <a href='https://video.advanceautoparts.com/' target='_blank'>Advance Auto Parts video subdomain<a/>

## Prominent names built on [Next.js](https://nextjs.org/showcase)
1. <a href='https://devices.netflix.com/en/' target='_blank'>Nike<a/>
1. <a href='https://m.staples.com' target='_blank'>Staples<a/>
1. <a href='https://hulu.com' target='_blank'>Hulu<a/>
1. <a href='https://m.twitch.tv' target='_blank'>Twitch<a/>
1. <a href='https://tiktok.com/en/' target='_blank'>TikTok<a/>

## Prominent names built on [Jekyll](https://jekyllrb.com/showcase/)
1. <a href='https://devices.netflix.com/en/' target='_blank'>Netflix Devices<a/>
1. <a href='https://developer.spotify.com/' target='_blank'>Spotify Developer page<a/>
1. <a href='https://bitcoin.org/en/' target='_blank'>Bitcoin<a/>
1. <a href='https://rubyonrails.org/' target='_blank'>Ruby on Rails<a/>
