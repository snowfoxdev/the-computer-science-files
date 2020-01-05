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
Infix: (f x)
Postfix: (x)f
```

Almost all programmers use prefix (function) notation, but LISP-based programmers are familiar with infix. The question: what are the costs and benefits of each?

## Prefix

One benefit of prefix is how currying looks/works:

```
f = x => y => z => x + y + z

f(1)(2)(3) // => 6
```
