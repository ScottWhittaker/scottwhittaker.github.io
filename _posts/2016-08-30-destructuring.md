---
layout: post
title:  Destructuring
date:   2016-08-30
categories: es6
---

> The destructuring assignment syntax is a JavaScript expression that makes it possible to extract data from arrays or objects into distinct variables.
<cite>[MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)</cite>


### Object Destructuring

What everyday problem does this address? A common thing to want to do is extract some data from an object, for example, a name and manufacturer from a starship object (I am always dealing with starships).

Given the following object (an example from [SWAPI](https://swapi.co/)) we would normally do something like this...

```js

const starship = {
    "name": "Millennium Falcon",
    "model": "YT-1300 light freighter",
    "manufacturer": "Corellian Engineering Corporation",
    "cost_in_credits": "100000",
    "length": "34.37",
    "max_atmosphering_speed": "1050",
    "crew": "4",
    "passengers": "6",
    "cargo_capacity": "100000",
    "consumables": "2 months",
    "hyperdrive_rating": "0.5",
    "MGLT": "75",
    "starship_class": "Light freighter"
}

// old skool
const starshipName = starship.name;
const starshipManufacturer = starship.manufacturer;

```

Nothing wrong with that but destructuring provides a different and more convenient way to achieve this...

```js

// new skool
const {name, manufacturer} = starship;
console.log(name, manufacturer); // Millennium Falcon, Corellian Engineering Corporation

```

Note the curly braces in the statement, this is the destructuring syntax...

`const` **{**`name, manufacturer`**}** `= starship;`

This creates 2 variables called `name` and `manufacturer` and assigns the values of `starship.name` and `starship.manufacturer` respectively. As long as the name of the variable is the same name as the object key the value will be assigned. [Try it out.](http://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=true&presets=es2015%2Ces2015-loose%2Ces2017&experimental=false&loose=false&spec=false&code=const%20starship%20%3D%20%7B%0A%20%20%20%20%22name%22%3A%20%22Millennium%20Falcon%22%2C%0A%20%20%20%20%22model%22%3A%20%22YT-1300%20light%20freighter%22%2C%0A%20%20%20%20%22manufacturer%22%3A%20%22Corellian%20Engineering%20Corporation%22%2C%0A%20%20%20%20%22cost_in_credits%22%3A%20%22100000%22%2C%0A%20%20%20%20%22length%22%3A%20%2234.37%22%2C%0A%20%20%20%20%22max_atmosphering_speed%22%3A%20%221050%22%2C%0A%20%20%20%20%22crew%22%3A%20%224%22%2C%0A%20%20%20%20%22passengers%22%3A%20%226%22%2C%0A%20%20%20%20%22cargo_capacity%22%3A%20%22100000%22%2C%0A%20%20%20%20%22consumables%22%3A%20%222%20months%22%2C%0A%20%20%20%20%22hyperdrive_rating%22%3A%20%220.5%22%2C%0A%20%20%20%20%22MGLT%22%3A%20%2275%22%2C%0A%20%20%20%20%22starship_class%22%3A%20%22Light%20freighter%22%0A%7D%0A%0A%2F%2F%20old%20skool%0Aconst%20starshipName%20%3D%20starship.name%3B%0Aconst%20starshipManufacturer%20%3D%20starship.manufacturer%3B%0A%0A%2F%2F%20new%20skool%0Aconst%20%7Bname%2C%20manufacturer%7D%20%3D%20starship%3B%0Aconsole.log(name%2C%20manufacturer)%3B%20%2F%2F%20Millennium%20Falcon%2C%20Corellian%20Engineering%20Corporation%0A%0A%2F%2F%20assign%20different%20names%0Aconst%20%7Bname%3Aship%2C%20hyperdrive_rating%3AhyperdriveRating%7D%20%3D%20starship%3B%0Aconsole.log(ship%2C%20hyperdriveRating)%20%2F%2F%20Millennium%20Falcon%200.5&playground=true)

For cases where you don't want to use the same name as an object key, for example you may not want your variable names to include underscores, you can assign to a different name with the following syntax...

```js

// assign different names
const {name:ship, hyperdrive_rating:hyperdriveRating} = starship;
console.log(ship, hyperdriveRating) // Millennium Falcon 0.5

```

#### Deeply Nested Objects

[Try it out](https://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact%2Cstage-2&code=const%20data%20%3D%20%7B%20%20%0A%20%20%22id%22%3A%221574083%22%2C%0A%20%20%22username%22%3A%22joebloggs%22%2C%0A%20%20%22counts%22%3A%7B%20%20%0A%20%20%20%20%20%22media%22%3A1320%2C%0A%20%20%20%20%20%22follows%22%3A420%2C%0A%20%20%20%20%20%22followed_by%22%3A3410%0A%20%20%7D%0A%7D%0A%0Aconst%20%7Bfollows%2C%20followed_by%3AfollowedBy%7D%20%3D%20data.counts%3B%0Aconsole.log(follows%2C%20followedBy)%20%2F%2F%20420%203410)

```js

const data = {  
  "id":"1574083",
  "username":"joebloggs",
  "counts":{  
     "media":1320,
     "follows":420,
     "followed_by":3410
  }
}

const {follows, followed_by:followedBy} = data.counts;
console.log(follows, followedBy) // 420 3410

```

#### Setting Default Values

[Try it out here](https://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact%2Cstage-2&code=const%20config%20%3D%20%7B%0A%20%20%20%20width%3A%20100%2C%0A%20%20%20%20height%3A%2060%0A%7D%3B%0A%0Aconst%20%7Bwidth%20%3D%2080%2C%20height%20%3D%2040%2C%20color%20%3D%20'red'%7D%20%3D%20%7B%7D%3B%0Aconsole.log(width%2C%20height%2C%20color)%3B%20%2F%2F%2080%2C%2040%2C%20red%0A%0Aconst%20%7Bwidth%3Aw%20%3D%2080%2C%20height%3Ah%20%3D%2040%2C%20color%3Ac%20%3D%20'red'%7D%20%3D%20config%3B%0Aconsole.log(w%2C%20h%2C%20c)%3B%20%2F%2F%20100%2C%2060%2C%20red) - notice how little code there is compared to the transpiled ES5 code.

```js

const config = {
    width: 100,
    height: 60
};

const {width = 80, height = 40, color = 'red'} = {};
console.log(width, height, color); // 80, 40, red

const {width:w = 80, height:h = 40, color:c = 'red'} = config;
console.log(w, h, c); // 100, 60, red

```

## Resources

- [You Don't Know JS: ES6 & Beyond: Destructuring](https://github.com/getify/You-Dont-Know-JS/blob/master/es6%20%26%20beyond/ch2.md#destructuring)
- [MDN - Destructuring assignment](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
- [Exploring ES6 - Destructuring](http://exploringjs.com/es6/ch_destructuring.html)
- [ES6 In Depth: Destructuring](https://hacks.mozilla.org/2015/05/es6-in-depth-destructuring/)
