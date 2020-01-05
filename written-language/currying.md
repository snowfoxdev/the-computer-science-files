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
  string => `${string} - count: ${string.length}` // `HELLO - count: 5`,
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

Let us use a mental item counter. Each variable identifier and each operation recieve one tickmark:

Currying: 3: `R.trim`, `R.toUpper`, ```string => `${string} - count: ${string.length}````
Non-Currying: 5: `string`, `trim`, `toUpper`, `newString`, ```return `${string} - count: ${string.length}````

Thus, if you believe in this type of mental counting, the Non-Currying way more more mentally taxing.

## Benefit and cost #3: readability

After becoming adapted to it, I think most people would say that composed functions are easier to read that regular functions because there are less mental items to keep up with. However, a function like `f(1)(2)(3)` can be difficult to read because one has to remember that `f(1)` returns a function that is run with `(2)` that returns a function that is run with `(3)`. This is even more difficult when not all arguments have been added yet. Having `f(1)(2)`, for example, could be confusing because one might think that this returns a regular value when it really returns a function.

Using infix function notation might fix this issue, though:

```
(((f 1) 2) 3) // => 6
```

Partial application:

```
(def x ((f 1) 2))
(x 3) // => 6
```
