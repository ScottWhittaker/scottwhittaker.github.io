---
layout: post
title:  Stumped by a Stub
date:   2016-05-16
categories: unit-testing
---

I was recently working on a codebase which had a huge test suite and when I added a simple new feature the tests broke. Great, this is what unit tests are for, to protect me from myself. However, when you don't practice [TDD](https://en.wikipedia.org/wiki/Test-driven_development) often in your day job you soon loose the habit, discipline and benefits that it affords. Thus I got stumped by a stub.

In short, all I did was add a new method onto an existing object, an event listener; a seemingly innocent modification. The test for this particular module failed as it was using a [Sinon stub](http://sinonjs.org/docs/#stubs) with the following API which stubs all the methods on a given object...

```js
var stub = sinon.stub(obj);
```

This is what the docs say about it...

> Stubs all the object’s methods. Note that it’s usually better practice to stub individual methods, particularly on objects that you don’t understand or control all the methods for (e.g. library dependencies). Stubbing individual methods tests intent more precisely and is less susceptible to unexpected behavior as the object’s code evolves.
> - [Sinon stub documentation](http://sinonjs.org/docs/#stubs)


## The Error

Before I understood what the issue was I was getting this error...

```shell
TypeError: obj.addEventListener is not a function
```

This is due to the fact that the object that I added an event listener on had been stubbed in the test suite (stubbed all the objects methods apart from the new one that I added in code). When the test ran, the code tried to execute the event listener on the object causing the test to fail as the stubbed object did not have the additional event listener defined on it.


## An Example

Here is a contrived example. First the code...

```js

// thing.js

export const thing = {

  init: function() {
    const widget = this.getWidget();
    widget.one();
  },

  getWidget: function() {
    return {
      one: function() {
        console.log('one');
      },
      two: function() {
        console.log('two');
      }
    }
  }
}

```

When `init` is called it creates a `widget` object and calls the method `widget.one()`. In our contrived test we want to ensure that `widget.one` gets called when the `init` method is invoked.

```js

// thing.spec.js

import test from 'tape';
import sinon from 'sinon';
import {thing} from './thing';

test('A contrived stub test', (assert) => {

  // wrap the existing getWidget function with a stub -
  // this means the existing function will not be called
  // and this stub will be called instead
  const getWidgetStub = sinon.stub(thing, 'getWidget');

  // stub out the methods for widget
  const widgetStub = sinon.stub({
    one: function() {},
    two: function() {}
  });

  // when get wigdet is called, return a stubbed implementation
  getWidgetStub.withArgs().returns(widgetStub);

  // if thing.init calls a function on the real widget that
  // has not been stubbed e.g. widget.three(), then the test
  // will fail as the widgetStub has not stubbed out a
  // method called three
  thing.init();

  const actual = widgetStub.one.calledOnce;
  const expected = true;

  assert.equal(actual, expected);
  assert.end();
});


```

This example test passes and represents the initial state of the module and associated test that I was working on. Now what happens when we change the implementation and add a new method on `widget`, `widget.three`, and we call that method in the `init` function?

Here is the modified code...

```js

// thing.js

export const thing = {

  init: function() {
    const widget = this.getWidget();
    widget.one();
    widget.three(); // <---- call additional method
  },

  getWidget: function() {
    return {
      one: function() {
        console.log('one');
      },
      two: function() {
        console.log('two');
      },
      three: function() { // <---- additional method added
        console.log('three');
      }
    }
  }
}

```

Running the test again results in an error...

```shell

➜  test-thing babel-node thing.spec.js | faucet
# A contrived stub test
/Users/anon/test-thing/thing.js:11
		widget.three();
		       ^

TypeError: widget.three is not a function
    at Object.init (thing.js:6:10)
    at Test.<anonymous> (thing.spec.js:27:9)
    at Test.bound [as _cb] (/Users/anon/node_modules/tape/lib/test.js:61:32)
    at Test.run (/Users/anon/node_modules/tape/lib/test.js:80:10)
    at Test.bound [as run] (/Users/anon/node_modules/tape/lib/test.js:61:32)
    at Immediate.next [as _onImmediate] (/Users/anon/node_modules/tape/lib/results.js:70:15)
✓ A contrived stub test [as _immediateCallback] (timers.js:368:17)
not ok 1 no plan found
not ok 2 no assertions found
⨯ fail  2

```

The fix is to add the new method to the stub as follows...

```js

// thing.spec.js

// stub out the methods for widget
const widgetStub = sinon.stub({
  one: function() {},
  two: function() {},
  three: function() {}
});

```

This is a brittle approach. As the object evolves over time this test is susceptible to breaking if a new method is added and called somewhere in code when the test is executed.

A better approach would be to use a different API and only stub the method you want to test...

```js
var stub = sinon.stub(object, "method");
```

I am still not clear on why on this approach was taken. The test in question had 6 assertions and did a lot of stubbing making it difficult to reason about in the first instance for me. Ideally the test could have been refactored to split those 6 assertions into individual tests making them easier to understand and maintain.


## Conclusion

- Keep tests simple
- Aim for a single assertion per test















