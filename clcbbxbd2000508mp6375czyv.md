# Variable Scope in JavaScript

There are three ways to declare variables in JavaScript - `let` , `const` and `var` . There are subtle differences in how they behave, and this post discusses one of them - their scope.

## What Is Variable Scope?

Imagine there are two rooms A and B, and you're in room A. With respect to room A, you're inside it and can be accessed within it. You're hence in the scope of A. With respect to room B, however, you're out of scope as you're outside the room and cannot be accessed within it.

Similarly, the variable scope is a block or region of code in which a variable can be accessed.

A simple example is given below. Try executing the code and notice the output. We'll get to the 'why' in the next section.

```javascript
function add(a, b){
	const c = a + b;
}

add(3, 5);
console.log(c) // Uncaught ReferenceError: c is not defined
```

## let & const

If you started learning JavaScript within the last few years and followed modern tutorials, chances are that you've only used `let` and `const` to declare variables. The difference between the two is that variables declared using `let` can be reassigned, while that declared using `const` cannot.

`let` and `const` follow what is called a **block scope**. A block is just any code within a pair of curly braces. You would have noticed blocks when defining functions, if-else statements, and loops.

Variables declared using `let` and `const` are only accessible within the block they are declared. Accessing them from outside the block will result in a Reference Error. Go back to the code snippet above. We have a function `add` inside which a variable `c` is declared. `c` is accessible within the code block of `add` but accessing it outside of the block throws an error.

An interesting thing to note is that you don't need to define functions or loops to create a block and implement block scope. You can use a pair of curly braces anywhere to create a block.

```javascript
{
  const fullName = 'Abin John'
  console.log(fullName.toUpperCase()); //ABIN JOHN
}

console.log(fullName.toLowerCase()) //error
```

## var

Before `let` and `const` were introduced to the JavaScript language with ES6 in 2015, `var` was the only way to declare variables. Like `let`, variables declared using `var` can be reassigned.

`var` follows what is called a **function scope**, which means that variables declared inside a function can be accessed from anywhere within it, but not outside it. At first glance, it might seem similar to block scope, but there is a subtle difference. I'll highlight that through an example.

Consider the snippet of code below that mimics the roll of a dice and displays a message depending on the dice value. The first snippet uses `const` for declaring the message variable. See if you can spot the error before running the code.

```javascript
function rollADiceES6() {
  const dice = Math.floor(Math.random() * 6 + 1)
  
  if(dice < 3){
  	const message = 'ES6: You shall not pass!';
  }
  else {
  	const message = 'ES6: You shall pass!'
  }
  console.log(message)
}
```

The error here is that `message` is defined inside the if and else blocks and hence, cannot be accessed outside of them. Kudos if you got it right! The next snippet uses `var` for declaring the message variable. Run this code and see what the output is.

```javascript
function rollADiceVar() {
  const dice = Math.floor(Math.random() * 6 + 1)

  if (dice < 3) {
    var message = 'var: You shall not pass!';
  } else {
    var message = 'var: You shall pass!'
  }
  console.log(message)
}
```

Surprised?

The code runs without any error because `var` isn't bound by the block scope. Curly braces don't affect the scoping of `var` variables. Variables declared inside a function will be accessible anywhere inside it. `message` is declared within the `rollADiceVar()` function and is accessible to the `console.log()` statement. Accessing `message` outside of the function will throw an error.

## A Bit More About Scope

### Global Scope

We learned about block scope, which is the region between two curly braces, and function scope, which is the function body. But what if variables are declared at the top level, outside of any block or function?

Such variables are bound by the global scope, which is a scope that's accessible everywhere. Therefore, **variables declared in the global scope are accessible from anywhere in the code**, irrespective of their declaration method. This is also the reason why these variables are referred to as *global variables.*

### Scope nesting

Nesting blocks and functions inside one another is a common practice in programming (nested if-else statements, helper functions inside bigger functions, etc.) We now know that every block and function has its own scope. When nesting is done, inner scopes have access to their outer scopes.

In the code snippet below, when the line `const b = a + 5` is encountered, JavaScript first searches for `a` in the inner scope. When not found, it searches the outer scope. In our case, `a` is present in the outer scope, but if it weren't present in the outer scope, JavaScript would keep traveling up the scope chain until the variable is found or the global scope is reached. If the variable is not found even in the global scope, a reference error is thrown.

```javascript
{
  const a = 23;
  {
    const b = a + 5;
    console.log(b) // 28
  }
   console.log(b) // error
}

{
  const c = a - 10; // error
  console.log(c)
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672413888163/3f293949-172f-49b0-bbdc-3b1a1450358f.png align="center")

> Scope chain only moves upwards i.e. an inner block can access the scope of the outer block, but not vice-versa. That's why line 7 in the above snippet throws an error. `b` is out of scope for the outer scope.

### Scope Priority

Consider the snippet below. The variable `firstName` is declared both as a global variable and inside the `whatIsMyName` function, while `lastName` is only a global variable.

```javascript
const firstName = 'Abin';
const lastName = 'John';

function whatIsMyName() {
  const firstName = 'Sam';

  console.log(firstName); // Sam
  console.log(lastName); // John
}

whatIsMyName()
```

In scenarios like these where the same name is given to variables of different scopes, the inner scopes have priority. `firstName` is present inside the function block, so its value is taken from there. `lastName` is not present inside the function block, so JavaScript moves one step up the scope chain to the global scope and finds `lastName` there. This order of priority is one reason why you should be mindful of variable names and ensure there are minimal conflicts. These bugs are tricky to catch as no error is thrown in most cases.

## How Should You Declare Variables?

It is considered best practice to use `let` and `const` in contrast to `var` to declare variables because their scoping behavior is similar to many other programming languages. The block scope is clean and less prone to errors. There are a few more reasons why the use of `var` is not encouraged anymore, like hoisting and re-declaration, but that's outside the scope of this article (pun intended!)

You can check out this [great post](https://javascript.plainenglish.io/4-reasons-why-var-is-considered-obsolete-in-modern-javascript-a30296b5f08f) on why `var` is considered obsolete to know more.

## Conclusion

Scoping might be a bit difficult to grasp at first. I had to sit on it for a few hours before I finally 'got it'. Play around with a few examples and you'll get it too. And remember to cut back on the usage of `var` and be conscious of variable names.

That wraps up this post! I hope you got to learn something new today. If you did, let me know in the comments or on [Twitter](https://twitter.com/abin_john98)