---
layout: post
title:  JavaScript Closures
date:   2016-03-10
categories: javascript
---

You can guarantee you will be asked about closures in an interview and if you are anything like me then you know what they are and how to use them (as you use them every day) but you forgot/cannot articulate it back to the interviewer. Here are a few succinct definitions to commit to memory...


> Closures are functions that refer to independent (free) variables. In other words, the function defined in the closure 'remembers' the environment in which it was created.
- [MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Closures)

> In a nutshell, a closure is the combination of a function bundled together (enclosed) with references to it’s surrounding state (the lexical environment). In JavaScript, closures are created every time a function is created, at function creation time.
- [Eric Elliot](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36#.b5xs2c1kj)

> Closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.
- [Kyle Simpson](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20&%20closures/ch5.md)