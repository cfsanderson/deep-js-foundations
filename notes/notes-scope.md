# Scope  

## Introduction
series intro  

## Scopes and Closures Introduction
- Nested Scope
- Hoisting
- Closure
- Modules  

## Understanding Scope
Scope: where to look for things

A variable shows up (incidentally only when we are adding something to it or getting something from it)

Questions

Scope is a compile time process NOT run-time. Think of it in 2 passes not 1.

JS has function scope only*

```js
var foo = "bar";

function bar() {
  var foo = "baz";
}


function baz(foo) {
  foo = "bam";
  bam = "yay"; 
  // bam (LHS) would create a global variable implicitly at run time unless in strict mode which would throw an error b/c it can't (reference error = "bam is not defined" [not UNdefined])
}
```

_*USE STRICT MODE!!!*_

Javascript programs are file-based.
Every file is in a separate program although they share the global window. If some are "use strict" and others aren't, no problem. Only a problem is if you are concatenating other bundled code into your own _*which you should not do*_.


```js
var foo = "bar";

function bar() {
  var foo = "baz";

  function baz(foo) {
    foo = "bam";
    bam = "yay";
  }
  baz();
}

bar();
foo;
bam;
baz();
```

LHS = Left hand reference = target of an assignment
RHS = right hand reference = NOT the target of an assignment

`bar()` on line 57 is not the TARGET of assignment so it has to be an RHS. It is actually 2 operations = 1) go resolve the `bar` expression to find out what value it is and then 2) do the () execution.

- Unfulfilled LHS refs like line 51 results in an implicit global var... which is BAD.
- Unfulfilled RHS refs like `baz()` on line 60 will create a reference error (i.e. undeclared).

Strict mode creates reference error for both.

The difference between `undefined` and undeclared is that an undeclared thing has never been declared. `undefined` means it has no value. You can assign `undefined` to a variable so that it does not currently have any other value. Undeclared just don't even exist.

Think of lexical scope (which is fixed, static, and happens at compile time) as a building that you progress up through if you do not find it in the current scope (bottom floor). If it isn't found on the top floor (global scope) it does not exist.

```js
var foo = function bar() {
  var foo = "baz";

  function baz(foo) {
    foo = bar;
    foo; // function...
  }
  baz();
};

foo();
bar(); // Error
```

A function DECLARATION (e.g. line 47) is named (RHS) and available to the enclosing scope.

A named function EXPRESSION (e.g. line 78) is not added to it's enclosing scope but will show up within it's own scope.

```js
var foo;

try {
  foo.length;
}
catch err {
  console.log(err); //TypeError
}

console.log(err); //ReferenceError
```

The `catch` clause declares a variable (err) that is block scoped to the catch clause itself. You cannot access `err` outside the catch block.

## Named Function Expression

```js
// anonymous function expression
var clickHandler = function(){
  // ..
};

// named function expression
var keyHandler = function keyHandler() {
  // ..
};
```

You should ALWAYS prefer the named function expression over the anonymous one.
- It provides a handy function self-reference
- 