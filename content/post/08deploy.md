+++
title = "Deploying my website using a git hook"
date = "2020-06-21T15:00:00+02:00"
category = "blogposts"
draft = false
+++

Inspired by [this post](http://ryanflorence.com/deploying-websites-with-a-tiny-git-hook/) by Ryan Florence, I set out to implement his idea (deploy using a git hook) for this blog.

I used to update this blog very little, and did that by logging in to the server, make the change, run `hugo` and then logout. For this year I have committed myself to write some more posts. Making deploying a bit easier should help a bit in achieving that goal.

Over the years I have become a git devotee and have put for example [my dotfiles](https://github.com/apie/dotfiles) in git (and online), and use git on every new project. I am also very fond of the [KISS](https://en.wikipedia.org/wiki/KISS_principle) principle and i would like to keep the amount of stuff that is needed on my server to a minimum. Maybe that's why at work I am always trying to reduce the amount of code :)

# Implementation
So how did I implement it? The basic idea was this:
- Write the blogpost locally: 
```sh
$ hugo new mypost.md
$ vim mypost.md
```
- Test locally by running `hugo server -D` (Build locally and show draft posts)
- If ok, commit the file and push to the server.
- The server updates itself using the `post-receive-hook` as described in Ryan's blog.
- The hook should also generate the site by running `hugo`, so i modified the hook to look like this:
```sh
#!/bin/sh
cd ..
GIT_DIR='.git'
umask 002 && git reset --hard && ./hugo
```

## Issues encountered
Well, first i had to copy over the site to my machine because I had been editing it on the server for all these years. Not that hard, just
```sh
$ rsync denickorg:~ . -av
```
Then, init a git repository and add the basic files (config.toml, content folder, static folder, .gitignore). Contents of the `.gitignore` (I keep the hugo binary in the same folder, but it should not end up in the git repository):
```
public/
themes/
hugo
```
Then, update the hugo versions on both ends ([just download the binary on their github](https://github.com/gohugoio/hugo/releases)). Now i had to update the theme I use ([liquorice](https://github.com/eliasson/liquorice/)]). I had to remove my folder `themes/` as it contained a lot of crap. Then add the theme back again:
```sh
$ git submodule add git@github.com:eliasson/liquorice.git
```


OK. All seemed fine, I wrote a draft blog post and committed it, but was greeted by the following message when I did `git push`:
```
remote: error: refusing to update checked out branch: refs/heads/master
remote: error: By default, updating the current branch in a non-bare repository
remote: error: is denied, because it will make the index and work tree inconsistent
remote: error: with what you pushed, and will require 'git reset --hard' to match
remote: error: the work tree to HEAD.
remote: error:
remote: error: You can set 'receive.denyCurrentBranch' configuration variable to
remote: error: 'ignore' or 'warn' in the remote repository to allow pushing into
remote: error: its current branch; however, this is not recommended unless you
remote: error: arranged to update its work tree to match what you pushed in some
remote: error: other way.
remote: error:
remote: error: To squelch this message and still keep the default behaviour, set
remote: error: 'receive.denyCurrentBranch' configuration variable to 'refuse'.
 ! [remote rejected] master -> master (branch is currently checked out)
 ```

 OK... So maybe this error was added after Ryan wrote his post (after all his post is 10 years old). Since we know what we are doing I'll just update the git configuration on the server as the error suggests. So I went to the repository directory and executed:
 ```sh
 $ git config receive.denyCurrentBranch ignore
 ```
 Then I tried again.. And it worked! In the output of `git push` we also see the output of Hugo.

# Conclusion
I'm quite happy with this setup. Writing a new post was made just a little bit easier. That's what I like, improve things/code step by step.

