---
layout: post
title:  Aurelia Router Demo
date:   2016-06-12
categories: aurelia
---

A demonstration of basic [routing](http://aurelia.io/docs.html#/aurelia/framework/1.0.0-beta.1.2.4/doc/article/cheat-sheet/7) with [Aurelia](http://aurelia.io/). Select the _**Profile**_ menu item on the navigation bar in the demo to see three levels of navigation that we will be creating with the [aurelia-router](https://github.com/aurelia/router).

<iframe class="iframe" src="https://gist.run/embed.html?id=92825f79a9156cd55194b2ba7c8c42df"></iframe>

## Route Configuration

By convention, Aurelia looks for a method called `configureRouter` on a view-model. This method gets passed two arguments...

- A configuration object: [`config`](https://github.com/aurelia/router/blob/master/src/router-configuration.js)
- A reference to the router: [`router`](https://github.com/aurelia/router/blob/master/src/router.js)

For example, the view-model for `app.js`, the top level navigation for the application, is a follows...

```js

// app.js

export class App {
  configureRouter(config, router) {
    config.title = 'Aurelia Router Demo';
    config.map([
      {route: ['', 'home'], name: 'home', moduleId: 'home/home', nav: true, title: 'Home'},
      {route: 'profile', name: 'profile', moduleId: 'profile/profile', nav: true, title: 'Profile'},
      {route: 'settings', name: 'settings', moduleId: 'settings/settings', nav: true, title: 'Settings'}
    ]);
    this.router = router;
  }
}

```

### Title

First an optional `title` property is set on the `config` object, this value is applied to the `<title>` element in the `<head>` of the html document. Note that the framework automatically handles this for you and appends that title to the title of the current route, in this case the default route `home`.

```html

<title>Home | Aurelia Router Demo</title>

```

### Routes

Next we create routes using the `config.map` method which expects an array of objects, each object defines a route. Route properties are described below as per the [documentation](http://aurelia.io/docs.html#/aurelia/router/1.0.0-beta.1.2.2/doc/api/interface/RouteConfig). Note that there are additional properties but only the ones used here are described.

- `route`: The route pattern to match against incoming URL fragments, or an array of patterns
- `name`: A unique name for the route that may be used to identify the route when generating URL fragments
- `moduleId`: The moduleId of the view model that should be activated for this route
- `nav`: When specified, this route will be included in the [[Router.navigation]] nav model. Useful for dynamically generating menus or other navigation elements
- `title`: The document title to set when this route is active

#### Setting the Default Route

['', 'home']: indicates this is the default route, `home is an alias`

The default route is defined by the empty string `''`, this is the route that will be loaded on application startup.

```js

{route: '', name: 'home', moduleId: 'home/home', nav: true, title: 'Home'}

```

This means when we navigate to the following address in the browser, the `home` route will be loaded as there is an empty string to the right of the hash tag.

```

http://localhost:9000/#

```

You can provide an alias for the default route if desired.

```js

{route: ['', 'home'], name: 'home', moduleId: 'home/home', nav: true, title: 'Home'}

```

This will allow you to navigate to the default route with either of the following url's...

```

http://localhost:9000/#
http://localhost:9000/#/home

```

If you require more than one alias you can do that too.

```js

{route: ['', 'home', 'welcome'], name: 'home', moduleId: 'home/home', nav: true, title: 'Home'}

````

```

http://localhost:9000/#
http://localhost:9000/#/home
http://localhost:9000/#/welcome

```

### Reference to the Router

Finally, notice that we create a reference to the router so we can access it in the corresponding view, `app.html`, allowing us to build navigation menus dynamically.

```js

this.router = router;

```

## Creating the View

Routes are defined in the view-model `app.js`. In the corresponding view, `app.html`, we require a `navigation` element and render it in the view, binding the `router` so it is available to it.

```html

<!-- app.html -->

<template>
  <require from="components/navigation.html"></require>
  <h1>Aurelia Router Demo</h1>
  <navigation router.bind="router" class="primary-navigation"></navigation>
  <div class="page-host">
    <router-view>
        <!-- Top level views are rendered here -->
    </router-view>
  </div>
</template>

```

### Dynamic Navigation Menu

In the `navigation` template, note that a `bindable` attribute is specified on the `<template>` element which provides a reference to the `router`. A repeater is used to iterate the `navigation` array on the `router` object. Each item in the `navigation` array is a [`NavModel`](https://github.com/aurelia/router/blob/master/src/nav-model.js) which corresponds to each of the routes configured in `app.js`. This provides access to properties such as `title` which is used for each menu item and `href` which is the url fragment i.e. the route that we want to navigate to e.g. `home`, `profile` and `settings`.

The `isActive` property is used to set a class name on the item that corresponds to the currently active route.

```html

<!-- components/navigation.html -->

<template bindable="router">
  <nav>
    <ul>
      <li repeat.for="row of router.navigation" class="${row.isActive ? 'active' : ''}">
        <a href.bind="row.href">${row.title}</a>
      </li>
    </ul>
  </nav>
</template>

```

This generates the following markup...

```html

<ul>
  <li class="au-target active" au-target-id="1">
    <a href.bind="row.href" class="au-target" au-target-id="2" href="#/">Home</a>
  </li>
  <li class="au-target" au-target-id="1">
    <a href.bind="row.href" class="au-target" au-target-id="2" href="#/profile">Profile</a>
  </li>
  <li class="au-target" au-target-id="1">
    <a href.bind="row.href" class="au-target" au-target-id="2" href="#/settings">Settings</a>
  </li>
</ul>

```

The text content of each `<a>` element is the value of the `title` property defined in the route configuration and the `href` attribute is populated with the url fragment as defined by the `route` property. Note that the first `<li>` element has a class of `active`, this denotes the active route allowing it to be styled accordingly.


## Child Routers

Armed with the knowledge of how to create the top level routes for an application there is not a great deal more to learn in order to create child routes. In fact the only difference in this example is how the default route is specified. First lets have a quick recap of the three levels of navigation in the example.

1. _Home / **Profile** / Settings_
1. _**Account** / Emails / Notifications_
1. _**Username** / Password_

When _**Profile**_ is selected in the menu, we route to `'profile/profile'` as defined by the `moduleId` for the `profile` route. If we take a look at `profile.js` we see that it is a view-model that implements the `configureRouter` method and defines 3 routes, this is the second level of navigation.

Note that the only difference from the top level route definition is how the default route is defined. We still use the empty string to denote the default route but a second property, `redirect`, determines which route to use.


```js

// profile/profile.js

export class Profile {
  configureRouter(config, router) {
    config.map([
      { route: '', redirect: 'account' },
      { route: 'account', name: 'account', moduleId: 'profile/account/account', nav: true, title: 'Account' },
      { route: 'emails', name: 'emails', moduleId: 'profile/emails/emails', nav: true, title: 'Emails' },
      { route: 'notifications', name: 'notifications', moduleId: 'profile/notifications/notifications', nav: true, title: 'Notifications' }
    ]);
    this.router = router;
  }
}

```

Similarly, when _**Account**_ is selected from the second level navigation we route to `'profile/account/account'`. The view-model for `account.js` also defines child routes creating a third level of navigation.

```js

// profile/account/account.js

export class Account {
  configureRouter(config, router) {
    config.map([
      { route: '', redirect: 'username' },
      { route: 'username', name: 'username', moduleId: 'profile/account/username/username', nav: true, title: 'Username' },
      { route: 'password', name: 'password', moduleId: 'profile/account/password/password', nav: true, title: 'Password' }
    ]);
    this.router = router;
  }
}

```

Both `profile.js` and `account.js` have their corresponding views which dynamically create the navigation menu.


## Thoughts

Routing is essential in any single-page application and is a hard problem to solve well. Aurelia appears to have solved it very well. Has it ever been easier to configure routes than this?

It would be straightforward to quickly flesh out the routes for a large application, something that has been painful with other approaches in my experience where a huge amount of boilerplate is required to hook everything together.

Of course there is a lot more to routing than is covered here. For example, hooking into application lifecycle methods where you can access route params or dynamically route to another view depending on a given condition, amongst other features. More **stuff** to do.
