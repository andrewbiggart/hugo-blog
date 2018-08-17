---
date: 2018-08-17T20:42:10+01:00
draft: false
title: "Deploy and host Hugo on Netlify"
author: "Klaudia Rozgonyiova"
description: ""
tags: [
    "tutorials",
    "webdev"
]
---
Hugo is a fantastic static site generator and if you're looking to move away from Jekyll, Hugo is an excelent choice. I'd say that the initial setup is much easier than Jekyll's and I'm nicely surprised how quick I was able to put this site together plus Hugo's blazing fast thanks to the fact it's built in in Go.

I tried Gatsby as well and maybe I chose the wrong theme to start with but both Jekyll and Gatbsy came short when it came to Hugo. I was not fan of all dependencies I needed for Jekyll and the plugin system in Gatsby.

The theme on this blog is [hugo-paper](https://github.com/nanxiaobei/hugo-paper) and I will show you how I set this up and host on Netlify. 

### Install hugo and create new project
Installing Hugo is as simple as this (for MacOS users, otherwise look up [Hugo website](https://gohugo.io/getting-started/installing/) for more instructions)
```bash
brew install hugo
```
After installing Hugo, simply run an another command which creates a new folder and adds project biolerplate.
```bash
hugo new site <siteName>
```

### Install a theme
The next step is to initialize git and install the theme you want to use. There is an [extensive list of themes](https://themes.gohugo.io/) for you to use. As mentioned above I am using `hugo-paper` ([github repo](https://github.com/nanxiaobei/hugo-paper)) which you can install like this:

```bash
cd <siteName>
git init
cd themes
git submodule add https://github.com/nanxiaobei/hugo-paper.git 
```

All that's left now is to add the theme name to `config.toml` file and create new content which you can do by adding `.md` files to `content/posts` folder and run `hugo serve` from root directory to serve it locally.

### Deploy with netlify
I am a huge fan of [Netlify](https://netlify.com/) (this very site is hosted there) so I will recommend you to deploy your newly made website there. They offer continuous delivery with Github so every time you push your changes to Github, the website is rebuild and deployed and you don't need to do anything. 

Easiest way to set up the commands is with `netlify.toml` file, simply create the file at the root. 
```toml
[build]
publish = "public"
command = "hugo --minify"

[context.production.environment]
HUGO_VERSION = "0.47"
HUGO_ENV = "production"
HUGO_ENABLEGITINFO = "true"

[context.split1]
command = "hugo --enableGitInfo"

[context.split1.environment]
HUGO_VERSION = "0.47"
HUGO_ENV = "production"

[context.deploy-preview]
command = "hugo --buildFuture -b $DEPLOY_PRIME_URL"

[context.deploy-preview.environment]
HUGO_VERSION = "0.47"

[context.branch-deploy]
command = "hugo -b $DEPLOY_PRIME_URL"

[context.branch-deploy.environment]
HUGO_VERSION = "0.47"

[context.next.environment]
HUGO_ENABLEGITINFO = "true"
```

On Netlify website, choose option to create `New site from Git` , choose your project and deploy. In theory, it should be as easy as that 🙌 

## Possible roadblocks
### Submodules error during the build
Ehm, if you didn't followed this tutorial or tutorial on offical Hugo website and you used `git clone` for your theme instead of `git submodule add` you might encounter a failed deploy. This is an easy fix, add your theme to your project with submodules, not cloning (check the process above).

### Editing theme
I have seen few articles about Hugo saying you can't edit the theme you added. It's because of the way submodules work, you reference the original theme from the original repo. I've seen suggestions that you fork the theme, edit it and then reference it, however, there are some disadvantages to this approach. I would say the biggest one is that you won't get a newer versions of the theme, which, in theory is not that big of a deal but still. There might be some useful updates that you will miss out if you fork the repo. 

There is a neater way, I believe, as Hugo can support several themes at once. You can edit them in `config.toml` file, 
```toml
theme = ["hugo-paper-edited", "hugo-paper"]
```

There is a theme inheritance algorithm that looks at the array above and create a new theme with theme on the left being loaded first and being the most important and merging the themes on the right into it. Hence, I copied the files I wanted to amend into a new folder called `hugo-paper-amended` and made all the changes to the theme there. The rest of the theme is then loaded from `hugo-paper` theme.

### Styles didn't load
You might get an error in the console saying that website wasn't served though secure `https`. That might be to a possible wrong `baseURL` value set in `config.toml`. If deployed already, Netlify supports https, so add your URL to the config and that will solve the problem!

## further reading

* [More on submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules)

* [Official Hugo quick start tutorial](https://gohugo.io/getting-started/quick-start/)

* [Hugo on Netlify](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/)

* [More about theme editing](https://gohugo.io/themes/theme-components/#override-template-files)
