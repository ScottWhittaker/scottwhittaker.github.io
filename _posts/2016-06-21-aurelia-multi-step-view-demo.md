---
layout: post
title:  Aurelia Multi-Step View Demo
date:   2016-06-21
categories: aurelia
---

I was curious as to how to create a multi-step form within a dialog with [Aurelia](http://aurelia.io/) and this is my first attempt. I deliberately resisted the temptation to look for an existing solution to force myself to try and solve the problem first before doing any comparisons. I have called it `multi-step-view` rather than `multi-step-form` as each view can be anything you like, it does not have to be a form at all. Also, it does not have to be rendered within a dialog, that was just my initial goal, it can be used anywhere. In fact, the demo does not show a dialog, if you want to see that in action take a look at the [repo](https://github.com/ScottWhittaker/aurelia-multi-step-view-demo).

In a nutshell, `multi-step-view` is a custom element that provides the ability to move back and forth between any number of views, each view can be validated before moving on to the next and you can cancel out at any point. Take a look at the demo...

<iframe src="https://gist.run/embed.html?id=c4b5282d6f43a7254d0a2eba9c1fa259" style="min-height: 800px;"></iframe>
