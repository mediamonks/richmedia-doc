---
layout: post
title: adding a bundleName
subtitle: So you define how your banner is called
author: mientjan
tags: [dataBind, inline]
comments: true
---

In the richmediarc you can now add a bundleName to the settings.

```json
{
  "settings": {
    "bundleName": "blue_160x600",
    "entry": {
      "js": "../../shared/script/main.js",
      "html": "../../shared/index.hbs"
    },
    "size": {
      "width": 160,
      "height": 600
    }
  },
  "content": {
    "title": "title blue",
    "type": "blue",
    "clickTag": "http://www.google.com"
  }
}
```

when running `npm run dev` or `npm run build` the unit will be build / dev in `blue_160x600`

```json
{
  "settings": {
    "bundleName": "${content.type}_${settings.size.width}x${settings.size.height}",
    "entry": {
      "js": "../../shared/script/main.js",
      "html": "../../shared/index.hbs"
    },
    "size": {
      "width": 160,
      "height": 600
    }
  },
  "content": {
    "title": "title blue",
    "type": "blue",
    "clickTag": "http://www.google.com"
  }
}
```

when running `npm run dev` or `npm run build` the unit will be build / dev in `blue_160x600` without being hard coded.

This is because `"${content.type}_${settings.size.width}x${settings.size.height}"` will resolve into `"blue_160x600"`