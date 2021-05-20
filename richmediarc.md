---
layout: page
title: .richmediarc
---
The .richmediarc file is one of the files that gets generated when you start a new project.

You can find it in the root directory of your creative, i.e. /src/300x250.

> _**Important:**_ Each creative requires at least one .richmediarc file

In a freshly generated project, the .richmediarc will look something like this

> _**Important:**_ File paths defined in the .richmediarc are ALWAYS relative to the .richmediarc itself.

## The content node

So everything in this node will be added and parsed by webpack. So lets look at how a default .richmediarc looks like.

```json
{
  "settings": {
    "entry": {
      "js": "./script/main.js", // required: points to the starting js file.
      "html": "./index.html" // required: points to the main html file.
      "html": "./index.hbs" // required: or you can point to a handlebar file
    },
    "size": {
      "width": 300, // required: width of richmedia unit
      "height": 600 // required: height of richmedia unit
    }
  },
  "content": {  // not required: can put anything in here.
    "bgcolor": "#FF0000", 
    "logo": "./img/logo.png" // not required: will be picked up by webpack and png minified.
  }
}
```

So if you want to load this file later you can do this like this.

```js
// you load the config like this. Why? because when accessing the config like this you 
// will get the parsed version of the config and not just the json file. 

import config from "richmediaconfig";

img.src = config.content.logo;
img.onload = () => {
    document.body.appendChild(img);
}
```

Why like this? `img.src = config.content.logo;` and not like `img.src = "./img/logo.png"` this is because the browser
will not be able to find ./img/logo.png.

As you can see this file contains configuration settings for your creative, as well as key-value pairs which you can use for content, styling or even functionality.

Size and height should already be set to the correct values based on the selection made during the generator process.

This file is parsed when you run “npm run dev”. Webpack grabs the values from this file and hardcodes them into the compiled creative which you see in the preview.

It’s possible to make changes to this file while the preview [server](./running-building-uploading.md) is running.

> **_Note:_** In older versions of generator-richmedia-temple changes made to the .richmediarc file require stopping 
> and restarting the dev server**                               

Go ahead and change content.bgcolor to something else and save the file. In the terminal, you’ll see the creative getting recompiled. Once done, you can refresh the preview page and see the changes you made reflected in the preview unit.

## Basic .richmediarc concepts

Below are some guides on how you can use these values in your creative.

### Using .richmediarc values in HTML

In your index.html, you can retrieve .richmediarc values using the `data-bind` attribute on HTML elements. This is 
made possible by the dataBind function which is imported by `./js/Banner.js`.

For example:

In .richmediarc:

```json
"content": {
...
	"cta": "Click here!"
}
```

In index.html:

```html
<body>
  <div class="cta" data-bind="text: cta"></div>
</body>
``` 

and

In .richmediarc:

```json
"content": {
...
  "bg-img-url": "../shared/images/background_300x250.jpg"
...
}
```

In index.html:

```html
<body>
  <img class="background-image" data-bind="src: content.bg-img-url"></div>
</body>
```

### Using .richmediarc values in HANDLEBARS
If you chosen handlebars as your html file you can use handlebars notation to access the richmediarc data. @see https://handlebarsjs.com/guide/#what-is-handlebars for more documentation about how to use handlebars. 

```

```

### Using .richmediarc values in CSS

Everything you put in the `.richmediarc` is accessible in css.

So the rich-media-temple-server (webpack) will add all the data from the config to the css through css vars.

How would you access them? Well like this.

```css
var(--{node}-{childNode}-{childNode})
```

For example:

In .richmediarc:
```json
"content": {
...
	"bgcolor": "#FFFFFF"
...
}
```
In style.css:
```css
body {
  background-color: var(--content-bgcolor);
}
``` 

and

In `.richmediarc`:

```json
"settings": {
	"size": {
    "width": 300,
    "height": 250
  }
}
```
In style.css:

```css
.banner {
  width: var(--settings-size-width)px;
  height: var(--settings-size-height)px;
}
```

### Using .richmediarc values in javascript

The main javascript file (conveniently named `./js/main.js`) imports the .richmediarc files as follows:

```js
import config from "richmediaconfig";
```
and passes this object into the Banner constructor

```js
const banner = new Banner(config);
```
Which also passes the config object to the Animation constructor.

From there, you are able to retrieve pretty much every value from the .richmediarc.

Example:

In .richmediarc:
```json
"content": {
...
  "intro": false
...
}
```  

In Animation.js:

```js
export default class Animation {
  constructor(container, config) {
    if (config.content.intro) {
      // play intro
    } else {
      // play main animation
    }
  }
}
```
## Advanced .richmediarc concepts

Below are advanced techniques you can use in the .richmediarc file.

### Inheritance

Sometimes you need to set up multiple .richmediarc files in the same creative. For example, when you’re working on a multilingual project and you want the English version to say “Click Here”, whereas the French version says ‘Cliquez Ici’.

In this case, the most elegant solution would be two .richmediarc files. For example:

`.richmediarc_en`

`.richmediarc_fr`

However, there will be a lot of overlap between these two files, if you’re only changing the copy - and not the background colors, the width height, the entry files, etc.

Our system supports inheritance of values, by providing a ‘parent’ file from which to inherit all the values defined in that file. The ‘child’ file can then overwrite only the needed values, thereby completely eliminating any overlap between the parent and child files.

Example

.richmediarc_en **(parent file)**:

```json
{
  "settings": {
  "entry": {
    "js": "./script/main.js",
    "html": "./index.html"
},
  "size": {
    "width": 300,
    "height": 250
  }
},
  "content": {
    "text": "Welcome!",
    "cta": "Click here",
    "bgcolor": "#FF0000"
    "bgimg": "./img/bgimage.jpg"
  }
}
```
.richmediarc_fr **(child file)**:

```json
{
  "parent": "./.richmediarc_en",
  "content": {
    "text": "Bienvenue!",
     "cta": "Cliquez Ici"
  }
}
```
As shown in the example above, in the French `.richmediarc`, we only specify the parent file, and the new values for text and cta. Everything else is inherited from the parent file.

This method is very useful and scalable, should the need arise to add even more languages or versions.

## Feeds
You are able to link to a Google spreadsheet, so you can build multiple units with one code base and one `.richmediarc`.
When you replace `API_KEY_PLACEHOLDER` with your own api key.

https://developers.google.com/sheets/api/guides/authorizing#APIKey

```json
{
  "settings": {
    "entry": {
      "js": "./script/main.js", // required: points to the starting js file.
      "html": "./index.html" // required: points to the main html file.
    },
    "size": {
      "width": 300, // required: width of richmedia unit
      "height": 600 // required: height of richmedia unit
    },
    "contentSource":{
      "url":"https://docs.google.com/spreadsheets/d/1cWkhMxC01WVFqWR616DyN7OPvtatD-ivguK9OSs_msg/edit?usp=sharing",
      "apiKey": "API_KEY_PLACEHOLDER"
    }
  },
  "content": {
    "copy1": "This is the first copy",
    "copy2": "This is the second copy",
    "bgcolor": "#FF0000", 
    "logo": "./img/logo.png"
  }
}
```

>**_Important:_** The Google sheet access needs to be set to public in order for this to work 

[Adjusting a banner](./adjusting-a-banner.md) or [Go to development server (Richmedia Temple Server)](./devserver.md)