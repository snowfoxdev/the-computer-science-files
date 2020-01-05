# Prefix, infix, and postfix function notation

**tl;dr** _How should functions look?_

In math, everyone learns infix notation for operators:

```
5 = 2 + 3
```

Then, computer scientists (and others) study and use postfix notation, more commonly known as Reverse Polish notation (RPN):

```
5 = 2 3 +
```

So, we have the three:

```
Prefix: + 2 3
Infix: 2 + 3
Postfix: 2 3 +
```

I would like to take this idea and apply it to functions:

```
Prefix: f(x)
Left infix: (f x)
Right infix: (x f)
Postfix: (x)f
```

Almost all programmers use prefix (function) notation, but LISP-based programmers are familiar with infix. The question: what are the costs and benefits of each notation?

## Currying

One benefit of prefix is how currying looks/works:

```
f = x => y => z => x + y + z

f(1)(2)(3) // => 6
```

Lambda calculus is also prefix:

```
(λxyz. x) a b c → a
```

In left infix, currying theoretically could look like the following. Pretend that the let function assigns its first argument (the identifier) to its second argument. Then pretend that the a function produces an anonymous function.

```
(let adder (a [x] (a [y] (a [z] (+ x y z)))))

(((adder 1) 2) 3)
```

What is unusual about currying in this way is that parentheses clusters (“)))))))))”) are usually close-parentheses, but for adder, it is open-parentheses that bundle up. Then right infix:

```
((((z y x +) [z] a) [y] a) [x] a) adder let)

(3 (2 (1 adder)))
```

Then, there is postfix:

```
x + y = z <= x <= y <= z = f

(1)(2)(3)f
```

Now, my analysis:

* **Prefix**
  * **Benefits:** In order to call the next anonymous function, one only has to append the next set of parentheses.
  * **Costs:** Like postfix, it can be hard to infer that each set of parentheses calls the anonymous function returned by what is to the left of the parentheses.
* **Left Infix**
  * **Benefits:** Anonymous functions returning anonymous functions is easier to infer for infix.
  * **Costs:** One has to add text to the beginning and end of an expression to call the next anonymous function. In addition, open-parentheses clusters ("(((((") show up, which is very unusual, as LISP-based languages usually only have close-parentheses clusters ("))))").
* **Right Infix** Anonymous functions returning anonymous functions is easier to read in infix.
  * **Benefits:** 
  * **Costs:**
* **Postifx**
  * **Benefits:** 
  * **Costs:**
