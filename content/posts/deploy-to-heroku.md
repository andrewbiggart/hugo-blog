---
title: "Deploy your app to Heroku"
date: 2018-08-15T20:38:50+01:00
draft: false
author: "Klaudia Rozgonyiova"
tags: [
    "webdev",
    "tutorials"
]
---

I am not deploying my apps very often (they usually end up almost finished somewhere in my local hell), so every time I find myself with the task to do so, the fairly simple process turns into the research and remembering commands I used in my previous lives. Hence, this guide.

I will show the process of deploying an Express app using MongoDB with Heroku step by step.

### Prerequisites


First, to deploy an app, you will need an app. I have one fairly simple REST API on my github already which we can use for this tutorial. [You can find it here](https://github.com/EffingKay/Count-Me-Up). Simply fork it and make local copy so we can make changes to it later.

To use Heroku, you will need git and heroku. I'm assuming we all have git on our machines so the next step is to install heroku CLI if you don't already have it. On Mac machines, simplest way is to install it with brew (for different OS refer to [heroku docs](https://devcenter.heroku.com/articles/heroku-cli) )

```
brew install heroku/brew/heroku
```

You will also need to create an account at Heroku website (free for up to 5 apps).

### Prepare your repo

Now, if you have a copy of the repo, we need to create `Procfile`. Procfile is a heroku specific file where you define commands that will run on your dyno. It should be called `Procfile` (no file extention) and should be localted in your root directory. I usually add one command to my file, using `forever` package which basically restart your app in case it crashes. Simply run these commands in the root directory of your app:

``` 
> npm i forever --save
> touch Procfile
```
Now, we need to add this to your Procfile.

```
web: ./node_modules/.bin/forever -m 5 index.js
```

Commit all your changes before moving on to the next step.

### Create heroku app

[Heroku docs](https://devcenter.heroku.com/articles/git) explain this process quite well and if you find any issues, I would recommend referencing them. But to put it simply, run the following command in your terminal.

```
> heroku create

Creating app... done, ⬢ afternoon-dawn-70065
https://afternoon-dawn-70065.herokuapp.com/ | https://git.heroku.com/afternoon-dawn-70065.git

> git push heroku master
```
This created the app and assigned it a name and URL, however, we still need to enable MongoDB.

### Install mLab add-on
Heroku offers free add-on for MongoDB and it's fairly easy to implement to your app. 

In `index.js` and also in `db/seeds.js`, we need to add `MONGODB_URI` to mongoose connect function, so I changed it to look like this:

``` 
mongoose.connect(process.env.MONGODB_URI || 'mongodb://localhost/polling');
```

Also in `index.js` change the port declaration on the last line to:

``` 
app.listen(process.env.PORT || 3000, () => console.log('Server has started'));
```

Commit your changes and push them to heroku ( `git push heroku master` ), then we can add mongolab with this command:

```
> heroku addons:create mongolab

Creating mongolab on ⬢ afternoon-dawn-70065... free
Welcome to mLab.  Your new subscription is being created and will be available shortly.  Please consult the mLab Add-on Admin UI to check on its progress.
Created mongolab-deep-94113 as MONGODB_URI
Use heroku addons:docs mongolab to view documentation
```

### Populate your database

I created the `seeds.js` file which populates your database with dummy data. You can run it and populate your Heroku database like this:

```
> heroku run node db/seeds.js 

collections dropped
... some not important warnings, bla bla
Candidates created.
Users created.
Done!

```


And now, if you go to `https://your-app-name.herokuapp.com/api/users` or `https://your-app-name.herokuapp.com/api/candidates` you should get a response (JSON with user and candidates info).


So yeah, it is as simple as that to deploy your app, it's just harder if it's your first time or if you haven't done it in a year or so. Thank you for reading!