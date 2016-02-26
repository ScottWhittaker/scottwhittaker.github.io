---
layout: post
title:  Apache 2.4 Upgrade - Authorisation Rules
date:   2015-12-12
categories: apache
---

When did Mac upgrade to use Apache 2.4, Yosemite, El Capitan? I only realised when my local web server returned a [403 Forbidden](https://en.wikipedia.org/wiki/HTTP_403) HTTP status code for an [alias](https://httpd.apache.org/docs/2.2/mod/mod_alias.html#alias) that I had not accessed for some time.

It turns out that the [authorisation](http://httpd.apache.org/docs/trunk/upgrading.html) rules have changed.

## Before

```apache
Alias /somedir "/Users/username/somedir/"
<Directory "/Users/username/somedir/">
        Options Indexes MultiViews
        AllowOverride None
        Order allow,deny
        Allow from all
</Directory>
```

## After

```apache
Alias /somedir "/Users/username/somedir/"
<Directory "/Users/username/somedir/">
        Options Indexes MultiViews
        AllowOverride None
        Require all granted
</Directory>
```

