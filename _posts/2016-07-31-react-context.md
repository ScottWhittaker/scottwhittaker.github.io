---
layout: post
title:  React Context
date:   2016-07-31
categories: react
---

This is what the docs have to say about context...

> Occasionally, you want to pass data through the component tree without having to pass the props down manually at every level. React's "context" feature lets you do this.
<cite>[Docs](https://facebook.github.io/react/docs/context.html)</cite>

A good use of context would be for passing state when using [redux](http://redux.js.org/docs/introduction/) with react for example. It is not recommended for passing props and should be used sparingly.

> Using context will make your code harder to understand because it makes the data flow less clear. It is similar to using global variables to pass state through your application.
<cite>[Docs](https://facebook.github.io/react/docs/context.html)</cite>

## Passing Props Manually

First lets take a look at passing props through the component tree manually...

<p data-height="600" data-theme-id="0" data-slug-hash="bZjQXY" data-default-tab="js" data-user="ScottWhittaker" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/ScottWhittaker/pen/bZjQXY/">React - passing props through component tree</a> by Scott Whittaker (<a href="http://codepen.io/ScottWhittaker">@ScottWhittaker</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

## Using Context

Now take a look at the same thing but this time using `context`...

<p data-height="600" data-theme-id="0" data-slug-hash="bZjONa" data-default-tab="js" data-user="ScottWhittaker" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/ScottWhittaker/pen/bZjONa/">React - passing info via context</a> by Scott Whittaker (<a href="http://codepen.io/ScottWhittaker">@ScottWhittaker</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Note that the example above is just for demonstration purposes and is not the recommended approach when simply passing props.
  
> Do not use context to pass your model data through components. Threading your data through the tree explicitly is much easier to understand. Using context makes your components more coupled and less reusable, because they behave differently depending on where they're rendered.
<cite>[When not to use context](https://facebook.github.io/react/docs/context.html#when-not-to-use-context)</cite>


