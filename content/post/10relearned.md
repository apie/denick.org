+++
title = "Things I (re)learned while programming a RSS newsfeed website"
date = "2023-12-31T16:00:00+02:00"
category = "blogposts"
draft = false
+++

I was annoyed by all the sports news in my newsfeed, so I set out to write my own.
<!--more-->

The basic idea was this:
1. combine a bunch of RSS feeds from news websites and write a frontend in which you can filter items or categories that you don't like.
2. should be technically as simple as possible
3. do not spend a lot of time on it

So I wrote a simple frontend using bootstrap and I used rss2json.com to easily read the rss feeds.

Learned:
- NOS.nl RSS feeds do not have a Last-modified header.
- There is such a thing as a stylesheet for a RSS feed ([example](https://feeds.nos.nl/nosnieuwsalgemeen)).
- Templating is included in HTML. Good because I dont want to use any framework.
- Naming a CSS class 'ignored', wont work! (instead I used the Dutch word)

(Re)Learned:
- HTML dataset/data attributes are very useful.
- HTML elements with id are directly accessible in javascript as a variable.

The stylesheet is not needed at all for my site, but still I wanted to have it working 😊. To make the browser understand the RSS stylesheet, the content type of the RSS file needs to be text/xml instead of the default application/rss. I forced this in Nginx like this:

```
location ~* \.rss$ {
   types { } default_type "text/xml; charset=utf-8";
} 
```

To deploy the code I just use one rsync command.

To hide clicked news items, I just use a cookie with an expiry of two days. For the settings I used local storage of the browser.

Final version
==
I realized I could simplify the site a bit more by not using rss2json.com. It adds a delay (not useful for news items) and it is not actualy needed since it is not hard to read RSS feeds in javascript.
So the final version consists of:
1. A list of RSS feeds I want
2. A cron job that combines the RSS feeds into one and serves this file (acting like a proxy)
3. A frontend that reads the combined feed, applies filtering and shows the items.

# Resources:
- Newsfeed source code: https://www.github.com/apie/nosfeed/
- RSS stylesheet: https://www.w3.org/Style/XSL/WhatIsXSL.html 
- HTML template tag: https://javascript.info/template-element
- HTML data attributes: https://javascript.info/dom-attributes-and-properties#non-standard-attributes-dataset
