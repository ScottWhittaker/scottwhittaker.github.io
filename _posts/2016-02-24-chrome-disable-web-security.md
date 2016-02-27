---
layout: post
title:  Chrome Disable Web Security
date:   2016-02-24
categories: chrome
---

Launching [Chrome](https://www.google.com/chrome/) and [Chrome Canary](https://www.google.co.uk/chrome/browser/canary.html) from the command line with web security disabled on OSX. Alias them in your `.bash_profile` and never worry about them again...

```sh

# chrome
# ------------------------------------------------------
alias chrome="/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --disable-web-security"
alias canary="open -a /Applications/Google\ Chrome\ Canary.app/Contents/MacOS/Google\ Chrome\ Canary --args --disable-web-security --user-data-dir"

```
