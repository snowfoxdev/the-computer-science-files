# Currying

Code is in JavaScript

This document proves the importance of currying. Currying 

```javascript
const f = x => y => z => x + y + z;

f(1)(2)(3); // => 6
```

Now, that is what currying is, but does currying dominate other forms?

## Benefit #1: delayed input of function arguments

Like so:

```javascript
const add = x => y => x + y;

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
const steve = blueDog('Steve');

jeff.name; // => Jeff
steve.color; // => blue

jeff.sayHello(); // log 'Hi, I'm Jeff. I am a dog, and I'm blue.'
steve.sayHello(); // log 'Hi, I'm Steve. I am a dog, and I'm blue.'
```

## Benefit #2: composition

One of the best examples of this is [Ramda](https://ramdajs.com/), which I am very much in favor of. For the sake of simplicity, I will only show left-to-right function composition because I find it easier to read. I do acknowledge though, that right-to-left function composition is what is used in math books. Left-to-right function composition is often called pipe, and I will use that name:

```javascript
const runNext = (index, functions, input) => 
  functions.length == index
  ? input
  : runNext(index + 1, functions, functions[index](input))

const pipe = (...functions) => input => runNext(0, functions, input)

const add = x => y => x + y;
const mul = x => y => x * y;

const celsiusToFahrenheit = pipe(
  mul(9/5),
  add(32)
);

celsiusToFahrenheit(20); // => 68
```

This is a contrived example. Some examples are not so contrived. Please consider the following using Ramda:

```javascript
const clean = R.pipe(
  R.trim, // => `hello`
  R.toUpper, // => `HELLO`,
  string => `${string} - count: ${string.length}`, // `HELLO - count: 5`,
);

clean(`      hello          `); // => `HELLO - count: 5`
```

To do the same without currying would look like:

```javascript
const clean = (string) => {
  const newString = R.toUpper(R.trim(string));
  
  return `${newString} - count: ${newString.length}`;
}
```

## Benefit and cost #3: readability

I think most people would say that `celsiusToFahrenheit = pipe(mul(9/5), add(32))` is highly readable. 
