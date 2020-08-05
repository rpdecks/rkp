---
title: "Blog Post 5 Md"
date: 2020-08-04T08:58:52-04:00
draft: true
---

What are 'Static Site Generators' (SSG's), what are their pro's and cons, and when should we use them?

In this post I will:
1. Explain what SSG's are
1. Cover their pro's & cons
1. Discuss good/bad use cases for them
1. List some popular SSG sites


## What are Static Site Generators (SSG's)?
The name here tells us a lot. The implication, obvious, that some sites are static while others are dynamic. In the case of static, think fixed or constant. This means that, just like creating a Word document and clicking save, developers create HTML content for a whole website (perhaps many pages) and save that version of the content. All of this content is then uploaded to a server where it can be delivered upon request, quite well by the way. More on that later.

Dynamic sites are more complex. As the name implies, these kinds of sites are changing, perhaps constantly. Think stock ticker, news, social media sites, etc. New tweets are published, then liked, then retweeted, etc. These kinds of sites are typically powered by a content management system (CMS) of some sort. These are sites like WordPress, Drupal, Joomla. This means that these websites are quite literally built upon request time and again for each request. On top of that, while you are viewing it, it is often changing dynamically as you scroll. For a WP site, it could just be a plugin with a widget that pull in/displays your company Google calendar. Dynamic sites usually have some sort of front end interface and back end database that lives on a server. When a user visits the site URL, data is fetched from the database and various other sources on the web and your view gets built for you and delivered to your browser.

Static site generators essentially take the static content (files) described in the first paragraph and build them into websites. This means files and folders are put onto a server and delivered upon request. It also means there is no database with complex server side processing to query the database and construct content for the user. Constructed content exists already and is merely served upon request from existing files and folders.

## Advantages of the SSG
1. **Speed**  
- A HUGE advantage. SSG's already have all the content locked, loaded, and ready to fire upon request while CMS type sites must run network requests to fetch data from API's (think pull breaking news stories from Reuters, etc), query the database to pull up records, and construct the pages you are to be served once you click go.  
- **53% of retail consumers will abadondon a website if it takes longer than 3 seconds to load** ([Research](https://www.marketingdive.com/news/google-53-of-mobile-users-abandon-sites-that-take-over-3-seconds-to-load/426070/)).  
- How? work of building is shifted away from time of request. This means the server only has to find and serve a file, perhaps in mass.  
built sites already, HTML response fast compared to CMS on demand build/respond. shifts build work away from time of request
leveraging CDN's and cached versions of the site, can be delivered optimally from closer locations in the network

1. **Scalable**   
- CDN's - Content delivery networks drastically optimize performance and user experience. How? They are geographically distributed group of servers which work together to provide fast delivery of Internet content. This means, the content is cached on servers around the globe and served to you from the closest one when you request it.
- **Simplified hosting** - Serverless in the sense of removing databases and layers of server side logic built around controlling/routing/building that content
- **cached content** - easier to scale than integreted logic communicating front to back etc.

1. **Relability**  
- **less moving parts** means less things to break. [Think CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) 

1. **Security** . 
- Since SSG's are rendered in advance, this leaves very few vectors for cyber attack. It is when we have data flowing from a front end to a back end database we are exposed to more intercept and attack and see things like database injections that lead to data breaches and security compromises.
- This is a big drawback for WordPress type CMS sites. These sites have thousands of plugins people use to do things like display the company calendar, recent blog posts, reviews, etc on their site. Thousands of plugins... some better than others. These are often subjects of malicious attack.
- WordPress type sites have admin panels that allow users to manage content and edit/maintain their website. Again, these get logins get hacked by bad actors.
- "There is no server more secure than the one that does not exist."

1. **Efficiency**  
- content delivery & speed == efficiency
- ease of maintenance mentioned below == efficiency
- inexpensives in terms of cost & resource impacts == efficiency

1. **Ease of maintenance**  
- **Host site anywhere:** can essentially be hosted in a Dropbox folder, AWS bucket, Github repo, etc).
- **Version control:** How nice to be able to rollback to any version of the site in GitHub commit history with a few keystrokes? Easy to fix errors. Not so easy with CMS. Databases can be irreversibly corrupted (ie. db DROP TABLE). This equals built in site back up.
- less time patching for security
- easier to debug (think front end back end bug tracing)
- faster, easier updates (think git push heroku master, wait, then db:migrate, then db:seed, etc...)
- edit content offline and push later

1. **Pick your langauge**   
- available utilizing many languagues
    * Ruby? (Jekyll, Slate...)
    * Javascript? (Next.js, Gatsby...) 
    * Go? (Hugo)
    * Pyton? (Pelican, MkDocs...)
    * etc... see [StaticGen](https://www.staticgen.com/)
1. Flexibility   
    - see **JAMstack**: 
      * modern web development architecture based on client-side JavaScript, APIs and Markup (JAM)
      * add JS to your site, fetch data from other API's real time and get some of the benefits of a dynamic CMS without the backend db (ie pull weather data, etc to effectively build your own widgets)
