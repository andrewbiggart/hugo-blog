---
title: "Accessibility Tips and Tricks"
date: 2018-08-15T20:58:39+01:00
draft: false
author: "Klaudia Rozgonyiova"
tags: [
    "webdev",
    "accessibility"
]
---

> In an ideal world it wouldn't matter, sadly everything has to be paid for. We have to justify development level to stakeholders as it costs money. Blanket accessibility would be nice, the pot isn't endless.

I read this comment and I vow to myself I will not rest until everyone thinks accessibility is easy. Ok, it is not but it's easier than many believe. Or maybe they already know and they're just too lazy to do it and look for excuses, I don't know. Anyway, accessibility is one of those topics that bear repeating (and I do, just check my twitter feed), so here we go. 


## Why code with accessibility in mind? 

Most of all, why not? You want your website to reach as many people as possible so why would you put your time into something that a lot of people can't use? Why would you not create something that your audience can enjoy, no matter their background? There is a lot that can be done, with minimal effort, to make your website easier to use to many, yet, we often neglect or are simply not aware of these easy steps. 

I also feel, that making your website accessible, is something that should be our responsibility as developers. We should strive to follow these steps by default, not wait until it becomes a project's requirement. So, let's start.

## Labeling and describing things
### Labels
There is very few things that make by blood boil like seeing a web form with no label, no name and placeholder doubling as label. Sure, it does look nice and is easier but you should still provide a concise label. If your design doesn't have labels, you can make them technically visible but practically hidden with few lines of CSS. `display: none` and `visibility: hidden` will hide content from screen readers, but I found a really nice trick how to overcome this.
``` css
.visuallyhidden {
  border: 0;
  clip: rect(0 0 0 0);
  height: 1px;
  margin: -1px;
  overflow: hidden;
  padding: 0;
  position: absolute;
  width: 1px;
}
```
You can found lots more tips and tricks about forms [here](https://www.w3.org/WAI/tutorials/forms/).

### Headings and links
Next thing that we need to be aware of are headings and links.Content must provide a logical and hierarchical heading structure, ie `h1` should be followed by `h2` which should be followed by `h3` etc. Links (external or navigational) must have a unique description about their function or target. One thing that I would point out is, don't just add a generic description when there is a lot of the same buttons on the page. Let's say we have a list of buttons that play different content. If you'd add `play` as description, the user wouldn't know what it is that the buttons plays. So, another useful code snippet:
``` html
<p>Content 1 title and description. 
    <a href="page1.html>More <span class="hidden">on content 1</span></a>
</p>
<p>Content 2 title and description. 
    <a href="page2.html>More <span class="hidden">on content 2</span></a>
</p>
<p>Content 3 title and description. 
    <a href="page3.html>More <span class="hidden">on content 3</span></a>
</p>  
```


## Focus on focus
### Content order and semantic HTML
I already started on this with heading but this is a much broader topic. Semantic HTML introduces meaning to the website, rather than just presentation. By providing semantic tags, you're providing additional info about the sections of your page. Therefore, use tags like `header`, `section`, `article`, `footer`, etc on your page and your users will be better for it. [Learn more about semantic HTML here](https://www.lifewire.com/why-use-semantic-html-3468271). 

### Focusable elements
To put it simply, all interactive element must be focusable and all non-interactive element must be not. This should not be a problem if you're following semantic HTML guidelines and are using HTML elements for the purpose they were intended, however, there might be an occasion when you need to change it. I'd look into `tabindex` attribute ([more info](https://www.w3schools.com/tags/att_global_tabindex.asp)). 

### Keyboard trap
.. is just what it sounds like. Imagine a scenario where you play a video in a new modal that you close by clicking on a background. Looks lovely, doesn't it? Now imagine that you don't have a mouse. How do you escape the modal? You don't. Always make sure that all actions can be performed by keyboard as well. 


## Think about colour
### Contrast
I may be shortsighted but I still can see very well and yet, even I became a victim of this one. I remember a loader that I saw the other day. It was a white company logo on light grey background. All was good when I visited the page on my new Mac in my bed late at night, but then I saw the same page on a work screen that had lower contrast and boom. No loader! Thing is, loader was still there. What made some people to design it this way, I will never understand.

### Test for different kinds of colour blindness

There is [several types](https://webaim.org/articles/visual/colorblind) of colour blindness. Familiarise yourselves with them and have them in mind when designing an interface. 

## Last bits and bobs

### no-text content

Everyone should know this one. Use `alt` attribute for images and find an alternative content for video and audio only content. [Have a little read on `alt` and how to use it properly here.](https://a11yproject.com/posts/alt-text/)

### language attribute
Add language attribute to html, like bellow to help screen readers to distinguish the languages. 
```html
<html lang="en">
...
</html>
```
If the site is written in one language, but the are certain words or phrases in different language, you can also help the readers with it.
```html
<p>Oh, this is this simply <i lang="fr">ennuyeux</i>, isn't it?</p>
```

## Further reading
* [This is pretty good article, I'd recommend.](https://medium.com/alistapart/writing-html-with-accessibility-in-mind-a62026493412)
* [And this is basically a bible on how to do accessibility like a boss.](https://a11yproject.com/)
* [And this *very* comprehensive (if a little daunting) checklist](https://www.wuhcag.com/wcag-checklist/)

## Last words
Like I said, there is quite a few things you can do that will not take too long and will help improve your website by A LOT. I believe that cost or money might be a factor but only in a legacy codebases. If you're building a new project, please keep these guidelines in mind and you're end up with a happier customers with minimal effort. 

I will try to make a new article, taking a website and showing the changes I did to improve accessibility in the near future. 

In the meantime, I recommend [this checker](https://achecker.ca/checker/index.php). Just enter the URL of your website and it will list all accessibility errors and how to fix them. 
