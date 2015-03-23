# Why TypeScript
There are two main goals of TypeScript:
* Provider an *optional type system* for JavaScript.
* Provide features of future JavaScript version to current JavaScript engines.

We find it best to explain these in separation in first go.

## The TypeScript type system

### Why Types?
* Increase you agility when doing refactorings
* Types are one of the best forms of documentation you can have. *The function signature is a theorem and the function body is the proof*.

### Your JavaScript is TypeScript
TypeScript provides compile time Type safety for your JavaScript code. This is no surprise given its name. The great thing is that the types are completely optional. Your JavaScript code `.js` file can be renamed to a `.ts` file and TypeScript will still give you back valid `.js` equivalent to the original JavaScript file. TypeScript is *intentionally* and strictly a superset of JavaScript with optional Type checking.

### Types can be Implicit
In order to give you type safety with minimal cost of productivity during new code development. E.g. TypeScript will know that foo is of type `number` below and will give an error on the second line as shown:

```ts
var foo = 123;
foo = '456'; // Error: cannot assign `string` to `number`

// Is foo a number or a string?
```
The motivation is that if you do stuff like this, in the rest of your code you cannot be certain that `foo` is a `number` or a `string`. Such issues turn up often in large multi-file code bases. We will deep dive into the type inference rules later.

### Types can be Explicit
As we've mentioned before, TypeScript will infer as much as it can safely, however you can use annotations to:
1. Help along the compiler, and more importantly the next developer who has to read your code (that might be future you!).
1. Enforce that what the compiler sees is, what you thought it should see. That is your understanding of the code matches an algorithmic analysis of the code (done by the compiler).

TypeScript uses postfix type annotations popular in other *optionally* annotated languages (e.g. ActionScript).

```ts
var foo: number = 123;
```
So if you do something wrong the compiler will error e.g.:

```ts
var foo: number = '123'; // Error: cannot assign a `string` to a `number`
```

We will discuss all the details of all the annotation methods supported by TypeScript in a later chapter.

### Types are structural
In some languages (specifically nominally typed ones) static typing results in unnecessary ceremony because even though *you know* that the code will work fine the language semantics force you to copy stuff around. This is why stuff like [automapper for C#](http://automapper.org/) exists. In TypeScript because we really want it to be easy for JavaScript developers, and be minimum cognitive overload types are *structural*. This means that *duck typing* is a first class language construct. An example is in order:

```ts
interface Point2D {
    x: number;
    y: number;
}
interface Point3D {
    x: number;
    y: number;
    z: number;
}
var point2D: Point2D = { x: 0, y: 10, }
var point3D: Point3D = { x: 0, y: 10, z: 20 }
function iTakePoint2D(point: Point2D) { /* do something */ }

iTakePoint2D(point2D); // exact match okay
iTakePoint2D(point3D); // extra information okay
iTakePoint2D({ x: 0 }); // Error: missing information `y`
```

### Type errors do not prevent JavaScript emit
To make it easy for you to migrate your JavaScript code to TypeScript, even if there are compilation errors, by default TypeScript *will emit valid JavaScript* the best that it can. e.g.

```ts
var foo = 123;
foo = '456'; // Error: cannot assign a `string` to a `number`
```

will emit the following js: 

```ts
var foo = 123;
foo = '456';
```

So you can incrementally upgrade your JavaScript code to TypeScript. This is very different from how many other language compilers work and is there for your ease.

### Types can be ambient
A major design goal of TypeScript was to make it possible for you to safely and easily use existing JavaScript libraries in TypeScript. TypeScript does this by means of *declaration*. TypeScript provides you with a sliding scale of how much or how little effort you want to put in your declarations, the more effort you put the more type safety + code intelligence you get. Note that definitions for most of the popular libraries have already been written for you by the [DefinitelyTyped community](https://github.com/borisyankov/DefinitelyTyped) so for the most purposes: 

1. The definition already exists 
1. You have a vast list of well reviewed TypeScript declaration templates already available.

As a quick example consider a trivial example of [jquery](https://jquery.com/). By default (as expect in good JS code) TypeScript expects you to declare (i.e. use `var` somewhere) before you use a variable
```ts
$('.awesome').show(); // Error: cannot find name `$`
```
As a quick fix *you can tell TypeScript* that there is indeed something called `$`: 
```ts
declare var $:any;
$('.awesome').show(); // Okay! 
```
If you want you can build on this basic definition and provide more information to help protect you from errors: 
```ts
declare var $:{
    (selector:string)=>any;
};
$('.awesome').show(); // Okay! 
$(123).show(); // Error: selector needs to be a string
```

We will discuss the details of creating TypeScript definitions for existing JavaScript in detail later once you know more about TypeScript (e.g. stuff like `interface` and the `any`).

## Future JavaScript Now
TypeScript provides a number of features that are planned in ES6 for current JavaScript engines (that only support ES5 etc). The typescript team is actively adding these features and this list is only going to get bigger over time and we will cover this in its own section. But just as a specimen here is an example of a class: 

```ts
class Point {
    constructor(public x: number, public y: number) {
    }
    add(point: Point) {
        return new Point(this.x + point.x, this.y + point.y);
    }
}

var p1 = new Point(0, 10);
var p2 = new Point(10, 20);
var p3 = p1.add(p2); // {x:10,y:30}
```

and the lovely fat arrow function: 

```ts
var inc = (x)=>x+1;
```

### Summary
In this section we have provided you with the motivation and design goals of TypeScript. With this out of the way we can dig into the nitty gritty details of TypeScript.

[](Interfaces are open ended)
[](Type Inferernce rules)
[](Cover all the annotations)
[](Cover all ambients : also that there are no runtime enforcement)


{% include "footer.md" %}