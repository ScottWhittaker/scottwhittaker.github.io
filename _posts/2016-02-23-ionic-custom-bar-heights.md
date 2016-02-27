---
layout: post
title:  Ionic Custom Bar Heights
date:   2016-02-23
categories: ionic
---

An [ionic](http://ionicframework.com/) demo app for testing custom bar heights. Allows you to add custom heights for subheaders, footers and subfooters without affecting the defaults. If you are not familiar with ionic you can [get started here](http://ionicframework.com/getting-started/).

See the [demo](http://scottwhittaker.github.io/ionic-custom-bar-heights) and test in [device mode](https://developer.chrome.com/devtools/docs/device-mode) for the best experience.

## tl;dr

Grab the [`_custom-bars.scss`](https://github.com/ScottWhittaker/ionic-custom-bar-heights/blob/master/scss/_custom-bars.scss) partial and include it in your app. Example templates can be found in [`index.html`](https://github.com/ScottWhittaker/ionic-custom-bar-heights/blob/master/www/index.html).

## Install

Clone the [repo](https://github.com/ScottWhittaker/ionic-custom-bar-heights).

```sh

npm install

```

Run the application.

```sh

ionic serve --lab

```

## Overview

By default the [ionic framework](http://ionicframework.com/) provides variables allowing you to override the heights of headers, subheaders, footers and subfooters. Those variables can be found in `lib/ionic/scss/_variables.scss`

```scss

$bar-height:                      44px !default;
$bar-title-font-size:             17px !default;
...
$bar-footer-height:               $bar-height !default;
$bar-subheader-height:            $bar-height !default;
$bar-subfooter-height:            $bar-height !default;

```

Overriding anyone of these variables may suite your needs assuming that you want any of the given components to have a consistent height throughout your application.

The implementation below builds upon this allowing you to define small and large bar components as well as the built in defaults. You can mix and match the components for any given view without affecting the default behaviour. For example, in a particular view you could have a large subheader, a default footer and small subfooter or any combination thereof.

View content is displayed between the header and footer components. The content element is positioned absolutely and takes into account which bar components are on screen. This is necessary in order to ensure that content is clipped in exactly the right place i.e. content will scroll under both header and footer components at the bottom and top boundaries of the header and footer bars respectively.

When using `<ion-header-bar>` and `<ion-footer-bar>` directives within an `<ion-view>` element the `<ion-content>` element has 2 classes applied to it, `.has-header` and `.has-footer`

```html

<ion-content class="scroll-content ionic-scroll has-header has-footer">

```

These classes are used to absolutely position the top and bottom of the ion-content element.

```css

.has-header { top: 44px; }
.has-footer { bottom: 44px; }

```

```

+-----------------------+
| Header 44px           |
+-----------------------+
| top: 44px             |
|                       |
|                       |
|                       |
| ion-content           |
|                       |
|                       |
|                       |
| bottom: 44px          |
+-----------------------+
| Footer 44px           |
+-----------------------+

```

When subheaders and/or subfooters are used you need to add a class(es) to the <ion-content> element so that the correct top and bottom values of the element can be calculated.

```html

<ion-content class="has-subheader has-subfooter">

```

```css

.has-subheader { top: 88px; }
.has-subfooter { bottom: 88px; }

```

```

+-----------------------+
| Header 44px           |
+-----------------------+
| Subheader 44px        |
+-----------------------+
| top: 88px             |
|                       |
| ion-content           |
|                       |
| bottom: 88px          |
+-----------------------+
| Subfooter 44px        |
+-----------------------+
| Footer 44px           |
+-----------------------+

```

The same convention will be used to allow for custom bar heights and subsequent calculation of content position. Note that the tab component is also considered and will work with custom height bars. Tabs may be positioned at the bottom or top of the application view as per style guides for iOS and Android.