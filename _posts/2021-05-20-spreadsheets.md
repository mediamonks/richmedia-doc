---
layout: post
title: using google spreadsheets in the feeds
subtitle: using google spreadsheets in the feeds
author: mientjan
tags: [dataBind, inline]
comments: true
---

So you have a richmedia unit and you need generate 19 different versions with different copy for different sizes and languages.

This is now possible with `contentSource` by adding contentSource to your richmediarc you can link to a spreadsheet and use that as a content source.

i created a example spreadsheet here. https://docs.google.com/spreadsheets/d/1BImA3lgTJsbVl56GQTFT-b1dx4FIoeQFBe9WLp0wWxc/edit#gid=0

as you can see it has 6 columns with the first row defining the name of the column. These rows with their content will be used as the source for 
the content property of your richmediarc.

```json
{
  "settings": {
    "bundleName": "${content.size}_${settings.version}_${settings.language}",
    "entry":{
        "js":"./script/main.js",
        "html":"index.hbs"
    },
    "contentSource":{
      "url":"https://docs.google.com/spreadsheets/d/1BImA3lgTJsbVl56GQTFT-b1dx4FIoeQFBe9WLp0wWxc/edit#gid=0",
      "apiKey": "API_KEY",
      "tabName": "main",
      "filter": {
        "version": "beauty"
      }
    }
  },
  "content":{
    "title":"default title",
    "copy0":"default copy",
    "cta":"default cta"
  }
}
```

You just need to replace API_KEY placeholder with your own generated key. You can generate a key here https://developers.google.com/sheets/api/guides/authorizing#APIKey

in contentSource you also see a property called tabName and filter.

 - `tabName` will let you define what tab you want to use of your spreadsheet.
 - `filter` will let you filter the only values you want. so in this example it will only select rows with the value "beauty" in the version column.
 
Try it out..