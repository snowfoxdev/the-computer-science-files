# Currying

Code is in JavaScript

This document proves the importance of currying. Currying 

```javascript
const f = x => y => z => x + y + z;

f(1)(2)(3); // => 6
```

Now, that is what currying is, but does currying dominate other forms?

## Benefit one: delayed input of function arguments

Like so:

```javascript
const add = x => y => z + y;

const addFour = add(4);
const addFive = add(5);

addFour(10); // => 14
addFive(10); // => 15
```

With this, I can create an almost object-oriented feel:

```javascript
const dog = color => name => ({
  name,
  color,
  sayHello: () => {
    console.log(`Hi, I'm ${name}. I am a dog, and I'm ${color}.`);
  }
});

const blueDog = dog('blue');
const jeff = blueDog('Jeff');
const steve = blueDog('Steve);

jeff.name; // => Jeff
steve.color; // => blue

jeff.sayHello(); // log 'Hi, I'm Jeff. I am a dog, and I'm blue.'
steve.sayHello(); // log 'Hi, I'm Steve. I am a dog, and I'm blue.'
```
