+++
title = "New website"
date = "2014-05-07T22:47:00"
categories = ['blogposts']
+++

So, here it is! My new website!

After running my custom coded, php powered website for **seven years**
it was time for something new. Actually there were many incarnations
before that one ;) I had been thinking about redesigning it many times
in the past year, but the thing that always held me back was the
content. There was not much on the website so why would I take the
effort to redesign it?

![A screenshot of my website up till 2014-02](/content/post/02newsite/site.png)

A screenshot of my website up till 2014-02.

Then somebody told me my website looked really old. At first I was
proud: *Ha let them talk, I like it!* But when I started thinking about
it I finally decided to give it a go. One thing I did not wanted was to
code a site from scratch; I wanted to experiment with a *static site
generator*. The most well known is [Jekyll](http://jekyllrb.com/) and
[Pelican](http://getpelican.com/) is also quite well-known. I went with
Pelican because of the simple fact that I wanted to host at my hosting
company and they did not provide a way to install Ruby things like
Jekyll, but Python things (Pelican) they did provide!

So, after researching a bit and generating a site at my workstation I
had figured out the basics. Now how to use it at the hosting company.
Actually, the only hint you need is: :

    easy_install --user

This installs Python packages in your user dir. I found the gallery
plugin for Pelican a bit cumbersome, so I'm leaving that for now. I set
up a development config and a publish config, to easily test the website
without breaking production. Also a nice hint is to specify :

    :status: draft

in your articles. They won't get published on the front page then.
Instead, you can access them from *drafts/*.

I'm quite content with this new format. Not much frills. I like
simplicity. And writing in ReStructuredText is like writing LaTeX, which
I'm already familiar with. I can write it on the server of my hosting
company in a simple text editor or in a different text editor when I'm
at my workstation.

Contents of new website
-----------------------

So, I have decided which items I wanted to copy from the old site:

-   Home page
-   Contact page
-   Public key

And the following item I will publish as a blog post:

-   Websites resume

So, I decided to drop the other two old items and the gallery, which was
already gone for some time.

While it is very well possible to blog using this website and layout, I
can't promise any regular updates. Some things I could blog about:
*linux tips and tricks, small scripts, random thoughts, things about
music/bands etc.* And one last thing: I decided to write this website in
English now, mostly to make it accessible to a larger audience.
