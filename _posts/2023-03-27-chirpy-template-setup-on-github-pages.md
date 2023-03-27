---
layout: post
title:  "Chirpy template setup on Github Pages"
date:   2023-03-27 21:20:58 +0100
categories: [Problem and solution, Blog]
tags: [jekyll, github pages]
---
When I began setting up this blog, I thought it would be quite simple and it would only take a moment. At least that's what the instructions were promising.
In reality, it turned into a short blog post, which I hope will be useful to someone. So here's the guide how to do it, how I did it.
## Get the Chirpy template
There are two ways to create a blog with this template. Both are described on the [**chirpy.cotes.page**](https://chirpy.cotes.page/posts/getting-started/) provided by the author, main contributor, therefore no sense of repeating it again.
The Wiki in the repository's readme leads to a deployed demo version of this template, where handful of tips on how to use it, can be found.
### Chirpy starter
A small codebase with several basic files, clean structure, ideal to just come and start building something.
As easy as two or three clicks of the mouse is forking this repository and setting as template for our own purposes.

> I highly recommend this option. It's easy and fast to start with.
{: .prompt-tip }

### Fork from repo jekyll-theme-chirpy
I went with this option at first, because I overlooked another, so much easier way. Took me some time to realise this. 

This codebase is more sophisticated. And it's not because of its size. It has bigger variety of elements. The js files, template files for pages, already built structure of these, combined, depending on each other.
I managed to run it locally and even deploy it, but in the end I got annoyed by bunch of things that weren't working. For example, the table of contents lost its animation effects. Or deploy scripts throwing errors at the beginnig, refusing to become green.
I've probably done at least few noobish mistakes, but that's how it is if you are completely unfamiliar with the tool you are using. 

> For more advanced users surely. Or users by accident choosing this option. Or everyone else who can't miss the opportunity to go the hard way :D
{: .prompt-warning }

## Install what's required to run Jekyll locally
As a prerequisite **Git** needs to be installed. Then there is docs link attached leading to Jekyll homepage with several more installation requirements, which are: 
### Ruby
From version 2.5 to everything below 3.0, will work. 
There happened to be some issues with github-pages gem used in Github Actions resulting in crashing pipelines for build and deploy if Ruby version was 3 or above. 
Yet, I see that my pipelines use Ruby 3.2.1 on Ubuntu 22.04 and report no errors or warnings in any of both stages. All green.
### RubyGems
Nothing unexpected here. Just follow the installation guide.
### GCC and Make
Windows users can totally skip this part. Their OS only needs Ruby and RubyGems. But I will still check for anything odd on my other machine,
that runs on Ubuntu. 

## Replace defaults in _config.yml
At this point all should be fine and set with your cloned template and Ruby environment necessary to launch Jekyll server.
To start Jekyll server and see the outcome, run in the terminal:
```
bundle exec jekyll s
```
Then scroll through _config.yml file. See which fields you can fill with your own blog data. 

## Deploy to Github Pages
It's almost done. Last thing you need to do is to deploy your build to Github.
Necessary pipelines are ready in `.github/workflows/pages-deploy.yml` file. To deploy your site you need to create a special branch for it. 
According to Github docs it has to have name `gh-pages` and be an orphan branch. 
To create such use command:
```
git checkout --orphan <branch name>
```
Orphan branch is unusual in several ways. It has no parents, no root, it is separated from history and commits of other branches in the project. 
Being such, all files that now will appear in staging area of the project should be removed, so branch could be truly clean:
```
git rm -rf .
```
To push it to remote, create init commit on it with for example README.md file. If empty, without any commit, orphan branch will be pushed
it will not be listed among branches when called `git branch`.
```
git commit -am "init commit of orphan branch"
git push –u origin <branch name>
```
And that's it. Branch is prepared. 
Last thing to do is to change a publishing branch, well documented with screenshots on [**github docs**](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site).

After few moments your static page should be available on github domain https://<<some-username-here>>.github.io.

Well, that's all :) 
