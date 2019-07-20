---
title: "Front End Checklist"
date: 2019-07-20T2:38:50+01:00
draft: false
author: "Klaudia Rozgonyiova"
tags: [
    "webdev"
]
summary: "What I check before releasing a website"
image: "https://images.unsplash.com/photo-1484480974693-6ca0a78fb36b?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1952&q=80"
---

Just before I call website done and abandon it, I like to go through these to make sure everything is in place.

## Other checklists

[https://codeburst.io/the-front-end-checklist-8b2292fdda44](https://codeburst.io/the-front-end-checklist-8b2292fdda44)

[https://github.com/thedaviddias/Front-End-Checklist](https://github.com/thedaviddias/Front-End-Checklist)

## HTML and CSS

- [https://github.com/joshbuchea/HEAD](https://github.com/joshbuchea/HEAD)
- [https://validator.w3.org/](https://validator.w3.org/)
- [https://www.10bestdesign.com/dirtymarkup/](https://www.10bestdesign.com/dirtymarkup/)
- [https://validator.w3.org/checklink](https://validator.w3.org/checklink)
- [https://uncss-online.com/](https://uncss-online.com/)
- [https://github.com/purifycss/purifycss](https://github.com/purifycss/purifycss)

## Accessibility checks

- [https://achecker.ca/checker/index.php](https://achecker.ca/checker/index.php)
- [http://contrast-ratio.com/](http://contrast-ratio.com/)
- [http://wave.webaim.org/](http://wave.webaim.org/)
- [https://websiteseochecker.com/html-sitemap-generator/](https://websiteseochecker.com/html-sitemap-generator/)

## Security checks

- [https://securityheaders.com](https://securityheaders.com/?q=https%3A%2F%2Fgotheory.netlify.com&followRedirects=on)
- [https://webbkoll.dataskydd.net/en](https://webbkoll.dataskydd.net/en)
- [https://observatory.mozilla.org/](https://observatory.mozilla.org/)
- [https://letsencrypt.org/](https://letsencrypt.org/)
- [https://www.ssllabs.com/ssltest/index.html](https://www.ssllabs.com/ssltest/index.html)

## Image optimisation

- [https://github.com/imagemin/imagemin](https://github.com/imagemin/imagemin)
- [https://tinyjpg.com/](https://tinyjpg.com/)

## Performance

- [https://tools.pingdom.com/](https://tools.pingdom.com/)
- [https://simplesharingbuttons.com/#intro](https://simplesharingbuttons.com/#intro)
- [https://developers.google.com/speed/pagespeed/insights/](https://developers.google.com/speed/pagespeed/insights/)
- [https://testmysite.withgoogle.com/intl/en-gb](https://testmysite.withgoogle.com/intl/en-gb)
- [https://www.webpagetest.org/](https://www.webpagetest.org/)
- add lazy-loading:


```javascript
// Preload resources you have high-confidence 
// will be used in the current page. 
// Prefetch resources likely to be used for future navigations 
// across multiple navigation boundaries.

`<link rel="prefetch" href="image.png">
<link rel="preload" href="image.png">`
```


## Last words
If you have any other tips and resources that you'd like to share, plese do. You can always ping me on twitter (I rarely check it) or don't hesitate to open a PR with newly added links, I welcome any additions :) You can find this article on github [here](https://github.com/EffingKay/hugo-blog/blob/master/content/posts/website-checklist.md).