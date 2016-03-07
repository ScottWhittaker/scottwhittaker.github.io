---
layout: post
title:  Aurelia - Data Driven Dynamic Views
date:   2016-03-07
categories: aurelia
---

If you are not familiar with [Aurelia][1] head on over to the documentation and take a look at the [Getting Started][2] section. It is a very compelling framework. I am willing to bet that if you have used other modern frameworks you will find Aurelia a breeze to get going and may want to use it as your framework of choice right now. Once you are up and running Aurelia has an almost intuitive quality thanks to its adherence to standards.

## Heterogeneous Views

Rendering different views into the [DOM][3] is straightforward with [Aurelia][1]. It is a common requirement to display a list or grid of items that differ in context and should have a different design so that users can easily distinguish between them. For example, when using your favourite catch up TV service you often find a grid with tiles of different sizes; larger tiles may infer a promoted or featured item, smaller tiles may be used for regular content. These views are often referred to as heterogeneous - _diverse in character or content_.

Take this example from [BBC iPlayer](http://www.bbc.co.uk/iplayer). It has a list on the left hand side, a large featured item in the centre and 2 smaller, regular content items on the right.

![Example content grid from BBC iPlayer](/img/heterogeneous-grid-iplayer.jpg)

## Example

Head over to this [Plunker][5] if you want see the example or dive into the code and mess around.

### Data

To demonstrate how to render different views based on data using [Aurelia][1] we will use the following [JSON][4] data to simulate a request to some API.

```json

{
  "citation": {
    "text": "Aurelia Docs",
    "url": "http://aurelia.io/docs.html"
  },
  "quote": "The compose Custom Element enables you to dynamically render UI into the DOM. Imagine you have a heterogeneous array of items, but each has a type property which tells you what it is.\r\n",
  "type": "quote"
}, {
  "title": "Standard Media Tile",
  "picture": "http://placehold.it/104x104",
  "description": "Cupidatat ad duis minim officia sint aute consectetur irure minim.\r\n",
  "type": "standard"
}, {
  "title": "Feature Media Tile",
  "picture": "http://placehold.it/424x208",
  "description": "Nisi occaecat ullamco consectetur tempor do nulla aute dolore eiusmod sunt eiusmod duis. Eu id non ipsum deserunt do enim et nostrud cillum ex ea magna deserunt est. Nostrud occaecat reprehenderit in velit veniam magna cupidatat dolor enim fugiat cillum. Lorem culpa exercitation ullamco elit culpa sit. Ut fugiat aliquip cillum mollit cillum tempor. Mollit veniam sint ipsum id nostrud adipisicing cillum tempor. Ex aliqua quis reprehenderit nostrud ullamco consequat.\r\n",
  "type": "feature"
}

```

Notice that there are 3 different values for `type:` `quote | standard | feature`. We will create an html template for each of these views as follows...

- `media-quote.html`
- `media-standard.html`
- `media-feature.html`

For each of these types we will render the corresponding html template in a list.


### Rendering the Views

We will assume we are rendering our list on a Home page. In [Aurelia][1] you create a `.js` file and a corresponding `.html` file with the same name in order to create a model and a view respectivley.

- `home.js` - _the model_
- `home.html` - _the view_


#### Model

In the model we fetch data in the `activate` function and assign it to a variable `data`. The `activate` function is one of the [Screen Activation Lifecycle](http://aurelia.io/docs.html#/aurelia/framework/1.0.0-beta.1.1.4/doc/article/cheat-sheet/7) hooks and is called before the the view is displayed. The `data` variable will be available to us in the view so that we can render data to the screen.

```js

// home.js

import {inject} from 'aurelia-framework';
import {HttpClient} from 'aurelia-fetch-client';

let url = './data.json';

@inject(HttpClient)
export class Home {

  data = [];

  constructor(http) {
    this.http = http;
  }

  activate() {
    return this.http.fetch(url)
      .then(response => response.json())
      .then((response) => {
        this.data = response;
    });
  }
}

```

#### View

```html

<!-- home.html -->

<template>
  <h2>Home</h2>
  <div repeat.for="item of data">
    <compose view="media-${item.type}.html" containerless></compose>
  </div>
</template>

```

In the view we use a repeater to iterate through the array of objects in `data` that was defined in the model. For each iteration, a local variable `item` is assigned the value of the current item in the array. In our example, this means that in the first iteration of the repeater, `item` will be assigned the following data...

```json

{
  "citation": {
    "text": "Aurelia Docs",
    "url": "http://aurelia.io/docs.html"
  },
  "quote": "The compose Custom Element enables you to dynamically render UI into the DOM. Imagine you have a heterogeneous array of items, but each has a type property which tells you what it is.\r\n",
  "type": "quote"
}

```

Notice the `compose` element in the repeater. The compose element enables us to include other views within our current view. For this example we use the `view` attribute to specify the name of the view and we determine the view file name by using the value of `item.type`. The `${}` syntax is string interpolation, this allows us to build up our file name dynamically from data.

```html

<compose view="media-${item.type}.html" containerless></compose>

```

Thus, for each iteration of the repeater a different view is 'composed' based on the `type` value defined in the data.

```html

<div repeat.for="item of data">
  <!-- 1st iteration -->
  <compose view="media-quote.html" containerless></compose>
  <!-- 2nd iteration -->
  <compose view="media-standard.html" containerless></compose>
  <!-- 3rd iteration -->
  <compose view="media-feature.html" containerless></compose>
</div>

```

Take a look at the [example][5].

## Conclusion

Nothing much more to say other than it is **very easy** to compose data driven views with [Aurelia][1].


[1]: http://aurelia.io/
[2]: http://aurelia.io/docs.html#/aurelia/framework/1.0.0-beta.1.1.4/doc/article/getting-started
[3]: https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model
[4]: https://en.wikipedia.org/wiki/JSON
[5]: https://plnkr.co/edit/qdiuiU?p=preview
