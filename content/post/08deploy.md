+++
title = "Deploying my website using a git hook"
date = "2020-06-21T15:00:00+02:00"
category = "blogposts"
draft = true
+++

Inspired by [this post](http://ryanflorence.com/deploying-websites-with-a-tiny-git-hook/) by Ryan Florence, I set out to implement his idea (deploy using a git hook) for this blog.

I used to update this blog very little, and did that by logging in to the server, make the change, run `hugo` and then logout. For this year I have committed to myself to write some more posts. This change should help a bit in achieving that goal.

Over the years I have become a git devotee and have put for example [my dotfiles](https://github.com/apie/dotfiles) in git (and online), and use git on every new project. I am also very fond of the [KISS](wikipedia TODO) principle and i would like to keep the amount of stuff that is needed on my server to a minimum. Maybe that's why at work I am always trying to reduce the amount of code :)

So how did I implement it? The basic idea was this:
- Write blogpost locally: 
```sh
hugo new mypost.md
vim mypost.md
```
- Test locally by running `hugo server -D` (Build locally and show draft posts)
- If ok, commit the file and push to the server.
- The server updates itself using the post-receive-hook as described in Ryan's blog.
- The hook should also generate the site by running `hugo`.

# Issues encountered
Well, first i had to copy over the site to my machine because I had been editing it on the server for all these years. Not that hard, just
```sh
rsync --verbose denickorg:~ . -av
```
Then, init a git repository and add the basic files (config.toml, content folder, static folder).
Then, update the hugo versions on both ends ([just download the binary on their github](https://github.com/gohugoio/hugo/releases)). Now i had to update the theme I use ([liquorice](https://github.com/eliasson/liquorice/)]). I had to remove my folder `themes/` as it contained a lot of crap. Then:
```sh
git submodule add git@github.com:eliasson/liquorice.git
```

<!-- more -->

