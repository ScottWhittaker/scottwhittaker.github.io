---
layout: post
title:  git - checkout previous branch
date:   2016-10-10
categories: git
---

Just discovered this little trick to checkout the previous branch you were working on. It allows you to toggle quickly between your last 2 branches...

```shell

git checkout -

```

For example, lets say you are working on `master` and you create a new feature branch `feature/foo`... 

```shell

git:(master) git checkout -b feature/foo
Switched to a new branch 'feature/foo'
git:(feature/foo)

```

To quickly switch back to `master`...

```shell

git:(feature/foo) git checkout -
Switched to branch 'master'
git:(master)

```

And again back to `feature/foo`...

```shell

git:(master) git checkout -
Switched to branch 'feature/foo'
git:(feature/foo)

```



