---
layout: post
title:  Chrome DevTools Copy Object
date:   2016-02-29
categories: chrome-devtools
---

Another one of those extremely useful tips that is easily forgotten if it is not something you do often. Maybe writing it down here will etch it in the brain?

- [Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools/)
- [Command Line Reference](https://developers.google.com/web/tools/chrome-devtools/debug/command-line/command-line-reference)
- [copyobject](https://developers.google.com/web/tools/chrome-devtools/debug/command-line/command-line-reference#copyobject)

When debugging it is common to dump out an object to the console and on occasions you may want to copy that object for some reason. For example, you may have to send it to a client to ensure it meets requirements.

Assume we have a JavaScript object as follows and we log it to the console during development...

```js

const data = {
    firstName: 'Peter',
    lastName: 'Beardsley'
};

console.log('data', data);

```

In order to copy the object to the clipboard from the console, right click on it and select **Store as Global Variable**. You will then see in the console that a global variable has been created for you named `temp1`. You can then use the `copy` function to copy `temp1` as a string representation to the clipboard.

Go ahead and open the console if you want to copy the object yourself. Mac: `Cmd + Opt + I`, Windows: `Ctrl + Shift + I`

### Step 1

Right click on the object in the console and select **Store as Global Variable**.

![Step 1](/img/chrome-devtools-copy-step-1.png)

### Step 2

A global variable named `temp1` has been created.

![Step 1](/img/chrome-devtools-copy-step-2.png)

### Step 3

Use the `copy` function to copy the object as a string to the clipboard - `copy(temp1)`

![Step 1](/img/chrome-devtools-copy-step-3.png)

### Step 4

Paste the value somewhere...

```sh

{
  "firstName": "Peter",
  "lastName": "Beardsley"
}

```

<script>
const data = {
    firstName: 'Peter',
    lastName: 'Beardsley'
};
console.log('data', data);
</script>