---
title: "Blog setup deep dive"
classes: wide
excerpt: "Why and how I set this blog up, how it's being made & run, and the beauty of hypertext. Also, some details on post-receive git hooks on Dreamhost."
---
**BLUF:** I'm using jekyll + dreamhost so I have ownership over content, no user exploitation, and relatively simple setup & deployment. Also includes technical details about how this site was set up.

## Rationale: Why a blog?
I like blogging. I find writing is important to clarify your own thoughts. Also, exposing ideas to the public (selfishly) helps me refine them[^1]. Also also, I've blogged before - for fun, at work, for business - and I've had good feedback. People have told me that they found my blogging useful. Now, I've got time, and I'm learning a lot about all kinds of things that I want to synthesise and share. So, the time is ripe.

[^1]: Ironic given I haven't set up comments yet. Just tweet at me in the meantime.

## Method: What's the best way to run a blog in 2019?
For better or worse, I still believe "put some HTML in a folder" is the best way for the internet to work, just as St. Berners-Lee intended. Make pages static, make URLs canonical, and use plenty of links. I'm happy with HTML5. CSS I will accept. JavaScript is for the weak. Come at me.

I'm also curmodgeonly enough to want to explicitly and unambiguously own my content. I don't want to let anyone put it behind a paywall, or have someone else's algorithm decide to disappear it. I also want it to be as simple as possible, technologically resilient, and use open standards. I don't think I need CDN acceleration or AMP support - it's a personal blog, not a webapp. Just put some HTML in a folder and serve it.

So I'm going the old-fashioned route. I'll make HTML files, and serve them. For tracking, I'll parse server logs. I won't have to worry about PHP versions or mysql users or tracking api keys or apis of any kind. All I'll have to worry about is keeping a web server working, and I'm old fashioned enough to have a web host for that. So now I just have to decide how to make the HTML, how to serve it, and how to deploy it.

### Why not Medium?
I don't mind Medium. I wrote 2 medium posts and it was super fast. I had signed up and was writing before I knew it, and had published as soon as I clicked "save". It looks nice - and you're not tempted to waste hours fiddling with how it looks. It's got built-in stats. It's got nice integration for sharing on all social media. It's SEO'd out the yin-yang. Critically, there's basically zero ongoing upkeep/maintenance overhead on you. Overall, it's a pretty good proposition. There's probably no better solution to get from zero to highly visible blog so quickly. So why not use it? Well, I might go back to it, if maintaining this gets too much. But I just don't like the business model. I want to own my content. I want to control it. I don't want to contribute to someone else's media empire. It's against the open web. If nothing else, it just rubs me the wrong way. 

### Why not Wordpress?
Honestly, I probably ought to be running Wordpress. It's fine. It's good, really. Certainly writing and publishing posts would likely be easier, and the plugin ecosystem is very robust. But it just feels like total overkill. I just prefer the idea of a static site - I'm not doing anything that needs to be dynamic, so why bother? I also have hygiene concerns around security and spam - I don't want to add to the pile of rotting, abandoned Wordpress installs for botnets to exploit 3 years down the line.

## Authoring: How do I make the HTML files?
Much as I love the open web and the magic of HyperText, I'm enough of a software engineer to hate HTML. It's ugly, verbose, fragile, repetitive, non-semantic, and mixes content with presentation. It's awful. So writing my own HTML for anything other than a one-pager is right out. 

But I'm pragmatic-ish. I recognise that the web has moved on, and not everything new is pure bloat. There are header `meta` tags I've never heard of but they seem to matter. I do want people to read this stuff, or at least have a fair chance of maybe seeing it. I want to be discoverable — without cheesy SEO. I do want to be shareable — but I don't need to track campaigns, open rates or engagement. I do want to see whether anyone's reading it, and if so, what they're reading — but I don't want to "track" anyone, or capture their data, and I don't care about dwell times or bounce rates.

So I need a modern system to help me write the HTML which I can put in a folder and serve.

### Jekyll + minimal-mistakes
I was already familiar with [Jekyll](https://jekyllrb.com/) and approve of its static site nature. I was planning to use it to generate the site. I came across [Claire Duvallet's blog](https://cduvallet.github.io) which I liked the look of. It uses the [academicpages theme](https://github.com/academicpages/academicpages.github.io), which was forked from the [minimal-mistakes theme](https://mmistakes.github.io/minimal-mistakes/), so I thought I'd start there. Turns out, the idea of a "theme" has progressed substantially from when I was last paying attention. It took me a solid 2 days to understand what was actually happening with the internal template dependency tree, and what I had to modify where to get what to happen. I was annoyed and frustrated. I was suckered in by tweaking. I was inspecting CSS border vs. padding values. I was colour-picking specific app brand colours. I thought I'd made a terrible mistake. But then I understood how it all meshed together, got the hang of it, and now it's fine. It looks nice enough for now and is fairly portable. I'm sticking with it for the forseeable.

### Pollen
[Pollen](https://docs.racket-lang.org/pollen/) "is a publishing system that helps authors make functional and beautiful digital books". I came across [Butterick's Practical Typography](https://practicaltypography.com) recently and loved everything about it. When I read about the system he created to create the ebook, I was thrilled. It has a lot going for it. I've dabbled in Racket before, I loved [Butterick's rationale and philosophy](https://docs.racket-lang.org/pollen/big-picture.html), the output looks beautiful, I was seduced by the beauty of the [x-expression](https://docs.racket-lang.org/pollen/second-tutorial.html#%28part._.X-expressions%29), and, to be honest, I was relishing the excuse to actually use a lisp for something practical. But, after finishing the tutorials, once I'd marvelled at the elegance and was trying to actually get a blog working, it was just too much lift. Especially when I saw the kinds of things the minimal-mistakes theme had baked in - social handles, opengraph post meta tags, etc. - and realising I'd have to code up all the CSS and boilerplate machinery from scratch… it was just too much. I'm sorry, Michael. I'll try again when I've got a book to write. If it helps, you've successfully made me feel bad for using Markdown.

## Hosting: How do I serve the HTML files?

I started with Github Pages, but realised I would need to use a 3rd party analytics tool to get any stats. I'd rather avoid that if I can - they're slow, rude, and encourage bloat. It also didn't quite scratch my ownership itch - who knows how long github pages will exist for?

I've been a happy Dreamhost customer for years. I don't come anywhere near any of my account space or bandwidth limits, and I'm already paying for it, so I'm just using that.  Yes, it's giving up some ownership, but I trust them. Dreamhost has one job that I pay it to do; that makes it more trustworthy than all the cute octocat logos in the world. Also, there's a copy of the whole site on the hard drive of my computer. If I really wanted to, I could serve it on a [solar powered raspberry pi](https://solar.lowtechmagazine.com).  

As for statistics, Dreamhost lets you download the Apache logs, which makes it easy enough for me to parse the logs myself to get page load stats, which is all I really care about, and then just for vanity. 

## Deployment: How do I put the HTML files on the server?
A git post-receive hook on the Dreamhost side. 

I thought about just building locally and rsync'ing the files across. I like the simplicity of that deployment, and might still go with it. It's the purest form of my "make HTML files and put them in a folder" mantra. However, I'm also all for having computers do the work — I'm happy to have the server do the building. Not sure this is a great idea; after all, now I have two Jekylls to maintain and keep in sync. But it's what the [official Jekyll documentation suggests](https://jekyllrb.com/docs/deployment/automated/), and it fits my workflow well enough as I'm committing & pushing my git repo already anyway.

### Setting up Jekyll on Dreamhost
I'm still not sure whether I had to do this, but I followed [the Dreamhost instructions on setting up rvm](https://help.dreamhost.com/hc/en-us/articles/217185247-Ruby-Version-Manager-RVM-) to install a specific version of Ruby in user-space. Jekyll was already installed and works fine (thanks Dreamhost!).

### Setting up the git post-receive deployment on Dreamhost
To set up the post-receive hook to build the site, I largely followed [the Jekyll instructions](https://jekyllrb.com/docs/deployment/automated/#git-post-receive-hook), but had to make a couple of tweaks. The final hook script I'm using is:

```bash
#!/bin/bash -l

# Install Ruby Gems to ~/gems
export GEM_HOME=$HOME/gems
export PATH=$GEM_HOME/bin:$PATH

GIT_REPO=$HOME/belowthenoisefloor.com.git
TMP_GIT_CLONE=$HOME/tmp/btnf
GEMFILE=$TMP_GIT_CLONE/Gemfile
PUBLIC_WWW=$HOME/belowthenoisefloor.com

git clone $GIT_REPO $TMP_GIT_CLONE
bundle install --gemfile=$TMP_GIT_CLONE/Gemfile
JEKYLL_ENV=production bundle exec --gemfile=$TMP_GIT_CLONE/Gemfile jekyll build -s $TMP_GIT_CLONE -d $PUBLIC_WWW
rm -Rf $TMP_GIT_CLONE
exit
```

Specifically I had to use the `--gemfile` option for the bundle commands to avoid a permissions error, and add the `JEKYLL_ENV=production` command to get certain features of the theme to come through. Also, contrary to the instructions, my version of git didn't have a copy of `post-receive.sample` to start from, so I had to create it from scratch and then remember to make it executable.

Now, I can make changes & write posts on my local machine. When they're ready, I can `git push deploy master`[^4] and ~10 seconds later everything's online. I'm writing, a computer program is writing the HTML files and putting them in a folder, and a web host is serving them. Lovely. It's not as seamless as typing into Medium's compose page and clicking "save", but it's not too far off.

[^4]: Strictly speaking, lest you fret, I've made a `deploy` alias that pushes to origin & deploy in the same command in fewer keystrokes.