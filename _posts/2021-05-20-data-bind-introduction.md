---
layout: post
title: dataBind() and why you should use it. 
subtitle: Introducing dataBind(model, document.querySelector('body')); 
author: mientjan
tags: [dataBind, inline]
comments: true
---

Data bind is a util in the mediamonks/temple to bind a object data to html.

In your index.html or index.hbs, you can easely bind js object to html elements with dataBind. 

For example:

doubleClick has a dynamic feed.

```js

  devDynamicContent.generator_richmedia_temple_framework_test_feed_main = [{}];
  devDynamicContent.generator_richmedia_temple_framework_test_feed_main[0]._id = 0;
  devDynamicContent.generator_richmedia_temple_framework_test_feed_main[0].id = 0;
  devDynamicContent.generator_richmedia_temple_framework_test_feed_main[0].reporting_label = "test_0";
  devDynamicContent.generator_richmedia_temple_framework_test_feed_main[0].default = true;
  devDynamicContent.generator_richmedia_temple_framework_test_feed_main[0].active = true;
  devDynamicContent.generator_richmedia_temple_framework_test_feed_main[0].version = ["0"];
  devDynamicContent.generator_richmedia_temple_framework_test_feed_main[0].text = "Welcome to this 0 Banner!";
  devDynamicContent.generator_richmedia_temple_framework_test_feed_main[0].logo = {};
  devDynamicContent.generator_richmedia_temple_framework_test_feed_main[0].logo.Type = "file";
  devDynamicContent.generator_richmedia_temple_framework_test_feed_main[0].logo.Url = "https://s0.2mdn.net/ads/richmedia/studio/33345/33345_20210113054941196_logo.svg";
  devDynamicContent.generator_richmedia_temple_framework_test_feed_main[0].cta = "Click here";
  devDynamicContent.generator_richmedia_temple_framework_test_feed_main[0].bgcolor = "#FFFFFF";
  devDynamicContent.generator_richmedia_temple_framework_test_feed_main[0].exit_url = {};
  devDynamicContent.generator_richmedia_temple_framework_test_feed_main[0].exit_url.Url = "http://www.google.com";

```

And you want to use these values in your html/hbs file.

you would only need to get the current object of the feed.

```js
const feed = window.dynamicContent["generator_richmedia_temple_framework_test_feed_main"][0];
```

and apply it to the html/hbs like this.

```js
// always import it
import dataBind from "@mediamonks/temple/util/dataBind";

// somewhere in your code
dataBind(feed, document.querySelector('body'));
```

and the html bindings would look like this

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>

<body>
  <div class="banner border" data-bind="style.backgroundColor: bgcolor">
    <div class="content fullscreen">
      <div class="title_container">
        <div class="title" data-bind="html: text"></div>
      </div>
      <img class="logo" data-bind="src: logo.Url">
      <div class="cta">
        <div class="cta_copy" data-bind="html: cta"></div>
      </div>
    </div>
    <div class="mainExit fullscreen"></div>
  </div>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.6.0/gsap.min.js"></script>
</body>
</html>
```

