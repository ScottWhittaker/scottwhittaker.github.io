---
layout: post
title:  Aurelia - Rating Dialog
date:   2016-05-30
categories: aurelia
---

I was working on a rating dialog recently so thought it would be interesting to see how [dialogs](https://github.com/aurelia/dialog) work in [Aurelia](http://aurelia.io/). The contents of a dialog are a `view` and `view-model` pair, the same as any other view in Aurelia, clean and simple.

Here is a working example...

<iframe src="https://gist.run/embed.html?id=5ff428cff8eedd0fdabe53d2cac00e8c"></iframe>

## Install and Configure

First install the `aurelia-dialog` plugin. See the [readme](https://github.com/aurelia/dialog) for full details.

```shell
jspm install aurelia-dialog
```

Add the plugin in `main.js`. This is where plugins are defined allowing them to be available throughout the application...

```js

export function configure(aurelia) {
  aurelia.use
    .standardConfiguration()
    .developmentLogging()
    .plugin('aurelia-dialog');

  aurelia.start().then(() => aurelia.setRoot());
}

```

## Creating the Dialog

As mentioned above, the contents of the dialog can be a view and view-model pair, in this case `rating-dialog.html` and `rating-dialog.js` respectively.

**rating-dialog.js**

Note that `DialogController` is imported and injected into the view model. It is then assigned to `this.controller` so that we have a reference to it in the corresponding view, which we will see below. This allows events in the view to invoke methods on the dialog.

Data is passed into the dialog via the `activate` method if required. This allows the view that is displayed within the dialog to be pre-populated with values. For example, address details may be passed in to be displayed in a form for editing. In this case a simple `rating` value is set to 1 if no value is provided.

```js

import {inject} from 'aurelia-framework';
import {DialogController} from 'aurelia-dialog';

@inject(DialogController)
export class RatingDialog {

  heading = 'Rate me...';
  maxRating = 5;

  constructor(controller) {
    this.controller = controller;
  }

  activate(rating = 1) {
    this.rating = rating;
  }

  rate(event) {
    if(event.target.dataset.rate) {
      this.rating = event.target.dataset.rate;
    }
  }
}

```

**rating-dialog.html**

Notice how the buttons are configured to call `cancel` and `ok` methods on the `controller` when actioned. Also note how `controller.ok(rating)` accepts the `rating` value. This provides the ability to pass data back to the view model that invoked the dialog.

```html

<template>
  <ai-dialog>
    <ai-dialog-header>${heading}</ai-dialog-header>
    <ai-dialog-body click.delegate="rate($event)">
      <div repeat.for="i of maxRating"
            data-rate="${i + 1}"
            class="rate ${i + 1 <= rating ? 'selected' : ''} ${i + 1 == rating ? 'active' : ''}">${i + 1}</div>
    </ai-dialog-body>
    <ai-dialog-footer>
      <button click.delegate="controller.cancel()">Cancel</button>
      <button click.delegate="controller.ok(rating)">Ok</button>
    </ai-dialog-footer>
  </ai-dialog>
</template>

```


## Using the Dialog



**app.js**

Note that `DialogService` is imported and injected into the view model. We also import the `RatingDialog` outlined above.  When the `rate` method is invoked (via a button in the corresponding view) the `open` method on `this.dialogService` is called with a configuration object specifying `viewModel` and `model`.

```js
this.dialogService.open({viewModel: RatingDialog, model: this.rating})
```


- `viewModel` is where we instruct the `DialogService` to use the `RatingDialog` as the view and view-model pair that will be displayed
- `model` is the data that we want to pass to the `RatingDialog` view-model

```js

import {inject} from 'aurelia-framework';
import {DialogService} from 'aurelia-dialog';
import {RatingDialog} from './rating-dialog';

@inject(DialogService)
export class App {

  rating = 3;

  constructor(dialogService) {
    this.dialogService = dialogService;
  }

  rate() {
    this.dialogService.open({viewModel: RatingDialog, model: this.rating})
      .then(response => {
        if(!response.wasCancelled) {
          console.log('OK');
          this.rating = response.output;
        } else {
          console.log('Cancel');
        }
        console.log(response.output);
      });
  }
}

```

The `DialogService` returns a promise when it is actioned (`ok`/`cancel`) with a `wasCancelled` property on the `response` object. Data is available via `response.output`.

**app.html**

The view displays the current rating. When the button is invoked it calls the `rate()` method on the view model and the dialog is displayed.

```html

<template>
  <button class="rating-button" click.delegate="rate()">Rate <span>${rating}</span></button>
</template>

```

## Conclusion

The `DialogController` and `DialogService` are easy to use and barely make an appearance in code. This seems to be typical of the Aurelia approach, things stay out of your way - testament to great design.

Given that the `DialogService` is just a 'wrapper' around a view and view-model pair then all manner of dialogs can easily be conceived. More investigation required.
