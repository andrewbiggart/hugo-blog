---
date: 2018-08-23T10:42:10+01:00
draft: false
title: "4 practices for better code"
author: "Klaudia Rozgonyiova"
description: ""
tags: [
    "concepts",
    "webdev"
]

summary: "Simple changes with huge impact"
image: "https://ak6.picdn.net/shutterstock/videos/9440576/thumb/1.jpg"
---
> "Clean code always looks like it was written by someone who cares."

I remember my first job as web developer. It was shortly after I gratuated from bootcamp and  I found myself in a role where every developer was on his own and the only reviews I've gotten were from designers who checked whether it matched the designs and behaved as expected. 

That was until I - inevitably - got stuck on one of the projects. One of the senior developers tried to debug my code and then he realized just how junior I was. In an attempt to make me a better developer and less of a pain in the ass for him, he came to me after, gave me a book and told me to read it. 

It was *Clean Code* by *Robert Cecil Martin*.

I can honestly say that this book is responsible for a huge shift in my thinking. I started to look at the code differently. Lots was said but few principles I will remember as long as I'll code. Here are the 4 that stuck with me the most and the ones I consider to be of the utmost importance to any developer or engineer. 


## Careful with the names
Be careful what you name your variables and functions, classes, files, basically anything and everything. Names should be descriptive of what the variables is or what the function does. This helps you read the code easier and it's more obvious what the code does on the first read without much investigation.

🤮***BAD***

```javascript
const a = document.querySelector('.a');
const b = document.querySelector('.b');
const w = window.innerWidth / 2;
const h = window.innerHeight / 6 ;

b.addEventListener('mousemove', (e) => {
  const x = e.clientX / w;
  const y = e.clientY / h;
  a.style.transform = `translate3d(-${mouseX}%, -${mouseY}%, 0)`;
  b.style.transform = `translate3d(${mouseX}%, ${mouseY}%, 0)`;
});
```

🌟***GOOD***

```javascript
const bg = document.querySelector('.background');
const bgTop = document.querySelector('.background-top');

const windowWidth = window.innerWidth / 2;
const windowHeight = window.innerHeight / 6 ;

bgTop.addEventListener('mousemove', (e) => {
  const mouseX = e.clientX / windowWidth;
  const mouseY = e.clientY / windowHeight;
  
  bg.style.transform = `translate3d(-${mouseX}%, -${mouseY}%, 0)`;
  bgTop.style.transform = `translate3d(${mouseX}%, ${mouseY}%, 0)`;
});
```


It shouldn't be too long, should be descriptive and the name format should be consistent across the code base.
<!-- {{< tweet 1029773908554276864 >}} -->


🤮***BAD***

```javascript
function setTheValueOfSomethingAndReturnAValueToDoSomething() { ... }
function addSomethingElseToThatThing() { ... }
```

🌟***GOOD***

```javascript
const getTime = () => { ... };
const getDate = () => { ... };
const setNewDate = () => { ... };
```

If there is a naming convention in the language, use it. Javascript's got `camelCase`, python is `using_underscores`, other language got theirs. Naming in line with conventions helps you and helps your fellow developers looking after the code base as well. 

## WET is bad, WET is bad aka DRY is gold
DRY - *Don't repeat yourself* is one of the best practices you can follow. If you find yourself WET: *Writing Everything Twice* and *Wasting everyone's time*, abstract it and make your live easier and code considerably shorter. 

🤮***BAD***

```javascript
const goodPractices = {
  dry: true,
  wet: false,
  kiss: true,
  descriptive: true
}

const getDRY = () => goodPractices.dry;
const getWET = () => goodPractices.wet;
const getKISS = () => goodPractices.kiss;
const getDescriptive = () => goodPractices.descriptive;

```

🌟***GOOD***

```javascript
const goodPractices = {
  dry: true,
  wet: false,
  kiss: true,
  descriptive: true
}

const getParam = (param) => goodPractices[param];
```
## Keep it simple, stupid

This tweet sums KISS principle better that I ever could:
{{< tweet 1030331822226501632 >}}
Basically, KISS is about making your code readable and simple. Adding only things you need, not things that look smart. Let's compare two solutions for one of the katas I found on Code Wars

🤮***BAD***

```javascript
H=(Q,S)=>Q.map(V=>null==V||(V.map?H(V,S):'object'==typeof V?H(Object.values(V),S):/nu|st/.test(typeof V)&(V=+V)==V&&++S[S[0]+=V,1]))
averageEverything=(...Q)=>H(Q,Q=[0,0])&&Q[0]/Q[1]
```

🌟***GOOD***

```javascript
const plus = (v,w) => v+w ;
const length = a =>
  typeof a==="number" ? Number.isNaN(a) ? 0 : 1 :
  typeof a==="string" ? Number.isNaN(Number(a)) ? 0 : 1 :
  typeof a==="object"
    ? Object.values( a || [] ).map(length).reduce(plus,0)
    : 0 ;
const sum = a =>
  typeof a==="number" ? a || 0 :
  typeof a==="string" ? Number(a) || 0 :
  typeof a==="object" 
    ? Object.values( a || [] ).map(sum).reduce(plus,0) 
    : 0 ;
const averageEverything = (...a) => sum(a) / length(a) ;
```

Now imagine there is a bug (and trust me, there will be bugs) and you need to fix it. Where do you even start with the top one? Sure, it looks genius and it's 3 lines, but at what cost? Meanwhile, the bottom example might be 10 lines longer but when you read it you have an idea of what's going on. I'm not saying ALWAYS make your code longer BUT you should try and make it as readable as possible.

## Longer is not always better

... when it comes to functions. I'm not saying you should keep your functions small no matter what if makes no sense or if it's just one line that return the sum of two numbers, but long functions usually mean that they do more than they should.

> "Functions should do something, or answer something, but not both."

Basically, if you see your function does something that can be potentially extracted and put into a separate function, you should do it. This also makes testing much easier. 

🤮***BAD***

```javascript       
const submitHandler = e => {
  e.preventDefault();
  postApiData('/login', {username, password})
   .then(user => {
    if (user && user.token) {
     Cookies.set('token', user.token)
   }
   return user;
   });
  if (!this.state.authError) {
   this.props.history.push('/theories')
  }
}
```

🌟***GOOD***

```javascript
const redirect = () => this.props.history.push('/theories');    
const login = (username, password) => {
  return postApiData('/login', {username, password})
    .then(user => {
      if (user && user.token) Cookies.set('token', user.token);
    });
}
    
const submitHandler = e => {
  e.preventDefault();
  login(username, password);
  if (!this.state.authError) redirect();
}
```

## Parting Words of Wisdom

Of course, all this is nice but don't take this as a law. Use your head and use this principles when applicable. I do hope this helps you on your journey to a better code!

{{< tweet 869762601642799104 >}}

<hr>
If you enjoyed the article, don't hesitate to share it on twitter!

<a 
class="twitter-share-button"
data-size="large"
target="_blank"
href="https://twitter.com/intent/tweet?text=4%20practices%20for%20better%20code%20by%20@EffingKay"
> tweet </a> <br>