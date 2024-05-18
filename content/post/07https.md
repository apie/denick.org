+++
title = "Configuring HUGO for https"
date = "2016-10-26T20:00:00+02:00"
categories = ['blogposts']
+++

This website is now available via https. Here is how I configured [Hugo](https://gohugo.io) to be compatible.
<!--more-->
When I opened the site over https, the stylesheet was not loaded because it was defined in the source to be located at `http://denick.org/..`. So something was not yet right.

Let's take a look at Hugo's config.toml:

    baseurl = "http://denick.org"

It was forced at http! What happens when we change it to be protocol agnostic?:

    baseurl = "//denick.org"

Regenerate the site (run `hugo`) and indeed: the website is now protocol agnostic, both http and https work.
The user is however forced to the https pages by this rewrite rule in `.htaccess`:

    RewriteEngine On
    RewriteCond %{HTTP:X-Forwarded-Proto} !http
    RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

That works :)

UPDATE (2020-06-21): I forced the baseurl to https since the agnostic url didnt work properly for the 'read more' link in the RSS feed. The website still works over plain http, but the stylesheets and all the hyperlinks are https. Should not be a problem.
