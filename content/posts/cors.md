---
date: 2018-08-15T20:42:10+01:00
draft: false
title: "CORS, preflighted requests & OPTIONS method"
author: "Klaudia Rozgonyiova"
description: ""
tags: [
    "concepts",
    "webdev"
]
---

Working on my [side project](https://gotheory.fun), I came across a problem familiar to many - CORS. However, it wasn't the usual message I get when I forget to enable cross-origin on my backend. This one said:
```html
Failed to load resource: Preflight response is not successful.
Fetch API cannot load <url>. Preflight response is not successful
```

This happened on the very first `GET` request I made to the server. I was fetching data from an API that was hosted on a different domain from my front-end. It was a simple request and the error left me very confused. After little troubleshooting, I found few solutions to my problem and learned quite a lot.


## CORS - Quick intro

CORS or _Cross-Origin Resource Sharing_ is a way for server to check if requests coming in are allowed if they're coming from a different origin. Meaning, if web application `xyz.com` makes a request to `something.io`, using either `XMLHttpRequest` or `fetch` API, CORS will use HTTP headers to tell the application if `xyz.com` has the right permission to access `something.io`. The permissions need to be configured at `something.io`, where you specify what methods and from what origins can come through. 

By default, for security, both `XMLHttpRequest` and `fetch` follow _same-origin_ policy so if you haven't specifically configured CORS and the HTTP headers on your back-end, the requests from different domain will fail.

## What is a preflight request?

When it comes to preflight, we can divide requests into two categories: _simple requests_ and _preflighted requests_.

#### Simple requests 
Some requests - called simple - don't trigger a preflight check. There is a simple exchange of CORS headers between client and server to check the permissions. For request to be simple, it needs to meet several criteria:

* The allowed methods are
    * `GET`
    * `HEAD`
    * `POST`

* The only headers which are allowed:
    * `Accept`
    * `Accept-Language`
    * `Content-Language`
    * `Content-Type` (but note the additional requirements below)
    * `Last-Event-ID`
    * `DPR`
    * `Save-Data`
    * `Viewport-Width`
    * `Width`

* There are only three values allowed for `Content-Type` header for simple requests
    * `application/x-www-form-urlencoded`
    * `multipart/form-data`
    * `text/plain`

* No event listeners are registered on any `XMLHttpRequestUpload` object used in the request; these are accessed using the `XMLHttpRequest.upload` property.
* No `ReadableStream` object is used in the request.

#### Preflighted requests 
Now, if the request doesn't meet the criteria above, the browser automatically sends a HTTP request _before_ the original one by `OPTIONS` method to check whether it is safe to send the original request. Most common cases are if requests have `DELETE`, `PUT` or any other method that can amend data, any headers that are not [CORS-safelisted](https://fetch.spec.whatwg.org/#cors-safelisted-request-header) (listed above) or `Content-Type` of, let's say `application/json`. 

So, let's say we send a `DELETE` request to the server. The browser sends `OPTIONS` request with headers containing info about the `DELETE` request we made.
```bash
OPTIONS /users/:id 
Access-Control-Request-Method: DELETE 
Access-Control-Request-Headers: origin, x-requested-with
Origin: https://example.com
```
The server then makes a check and if it allows the request, it responds with status code `200 OK`.
```bash
HTTP/1.1 200 OK
Content-Length: 0
Connection: keep-alive
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: POST, GET, OPTIONS, DELETE
```
After this check, the browser will send the original `DELETE` request we made in the first place. 

## The error and how to fix it
You have several options, really. The easiest option would be to avoid the preflight request altogether by making sure your request falls into the `simple` category. Go through your settings and headers and remove or change any complex headers that aren't needed. 

However, this only solves small percentage of cases. My app is authenticated and I'm sending `Authorisation` header with tokens by `PUT` and `DELETE` methods. I am using [cors](https://www.npmjs.com/package/cors) npm package that enables CORS in my Express app so I checked their documentation and they also provide `OPTIONS` handling. All's fixed just with one line.

### Final words
If you want to learn more, I'd recommend the MDN's articles on [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) and [OPTIONS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/OPTIONS). [This article](https://damon.ghost.io/killing-cors-preflight-requests-on-a-react-spa/) was very useful when I was trying to eliminate preflight, despite some advices like `Moving Authorization to the query string` seemed little extreme and for some reason very uncomfortable.