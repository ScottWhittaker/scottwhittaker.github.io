---
layout: post
title:  ZSH - Advanced History
date:   2016-04-20
categories: zsh
---

I have been using [oh my zsh](https://github.com/robbyrussell/oh-my-zsh) for a few years now and did not know about the advanced history feature provided by [zsh](https://en.wikipedia.org/wiki/Z_shell). I found out about this handy little tip via [Command Line Power User](http://commandlinepoweruser.com/) by [Wes Bos](http://wesbos.com/), a free video series about using the command line.

## History Lesson

The `up` and `down` arrow keys are used in order to cycle through recent commands in the terminal. This is handy but what if you are looking for a specific command that was entered an hour ago perhaps? In this case you would have to press the `up` key multiple times until the command you are looking for is found.

For example, lets say an hour ago we fetched some data via the command line from [SWAPI](https://swapi.co 'The Star Wars API') using [HTTPPie](https://github.com/jkbrzt/httpie) as follows...

```shell
http swapi.co/api/
```

```shell
http swapi.co/api/planets/1/
```

An hour passes and you would like to run the same command(s) again, but in the intervening time you may have entered 20 other commands. Instead of pressing the `up` key 20 times, you can type part of the command you are looking for and then hit the `up` key. For example, type...

```shell
http
```

Now press the `up` key and notice that the last command beginning with `http` is loaded for you. Keep pressing `up/down` to cycle through recent `http` commands.

```shell
http swapi.co/api/planets/1/
```

```shell
http swapi.co/api/
```

Note that you do not have to type the full command `http`, you can shorten that to `ht` or even `h` to get recent commands beginning with those characters.

## Conclusion

How did I not accidentally discover that feature? I will be saving myself a few seconds per day now.


