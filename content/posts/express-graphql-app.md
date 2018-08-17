---
title: "Step-by-step: Express, GraphQL & MongoDB backend"
date: 2018-08-16T20:38:50+01:00
draft: true
author: "Klaudia Rozgonyiova"
tags: [
    "webdev",
    "tutorials"
]
---


In this tutorial I will show how to build a *very* simple server app using Express and GraphQL with MongoDB database and deploy it to Heroku.

You can check out the [final app here](https://polar-dawn-98612.herokuapp.com/graphql) and the [source code on my github](https://github.com/EffingKay/eavesdropper-server)

#### Prerequisites 
I will show how to install most dependencies but you will need to have node and npm installed on your machine and [MongoDB](https://docs.mongodb.com/manual/installation/) as well. 

*Optional:* You'll also need a Heroku account if you don't have one yet and want to deploy your app, we'll only use free tier and all add-ons are free as well. 

#### Few words before we begin
I've seen GraphQL mentioned one too many times on Twitter so I decided to check it out and as with any technology, I believe the best way to learn it, is to build. The app we will be building is the most basic I could think of. I like to learn concepts and how things work on a smaller scale before I move onto more complex ideas. If you're new to GraphQL and/or Mongo, this is a great starting point.

## Step-by-step
### Initialize your project and install dependencies

I learned a neat little trick recently and that's `npm init -y` where the `-y` flag answers all the questions you're asked when creating `package.json`. Saves you about .3s but who cares, I love it  ¯\ _(ツ)_/¯ 

```bash
mkdir simple-server-app
cd simple-server-app
npm init -y
npm i --save express express-graphql graphql nodemon
```

We need to install a few packages - `express`, `express-graphql` and `graphql` needed for our server and `nodemon` package which is not necessary but makes development easier as it restart the server on every saved change so you don't have to.

If you're gonna deploy the app, I recommend `cors` package as well. (maybe add link to cors article)

### Create a server
``` bash
touch index.js
mkdir config
touch config/config.js
```
<small>*Note:* You don't need to create config in a seperate folder and file, this app is simple and all config can be contained inside `index.js` but it'is just my preference to keep the config values separately. </small>

```javascript
const port = process.env.PORT || 4000;
module.exports = {port};
```
First, let's configure our `PORT` variable inside `config/config.js`. This defines port on which the app is running, either when deployed or locally on `localhost:4000`. With this in place, we can start creating the server inside `index.js`. After we import all the packages we need, we define our server.
```javascript
const express = require('express');
const config = require('./config/config');

const app = express();

app.listen(config.port, () => {
    console.log(`Now listening on ${config.port}`);
})
```
Run `nodemon` or `node index.js` command in your terminal and you should see message saying `Now listening on 4000`. With basic server in place, let's add GraphQL. 