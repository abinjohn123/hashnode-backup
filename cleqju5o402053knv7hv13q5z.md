# Pure and Impure Functions in JavaScript

"Pure" and "Impure" are two terms that are often associated with functions in JavaScript, especially in React Components. This post explains what they are, how they differ, and whether you should try to write pure functions all the time.

## Purity and Impurity

A function can be classified as pure or impure depending on two factors

1. It must be predictable. Same input = same output.
    
2. There should be no side effects from calling it.
    

### Predictability

A function is predictable if, for a given input, the function call produces the same result all the time.

Consider the code snippet below. The function `add()` will always return 8 if the arguments are `3` and `5`. This function is predictable.

```javascript
const add = (a, b) => a + b;

console.log(add(3, 5)); // 8

console.log(add(3, 5)); // 8

console.log(add(3, 5)); // 8
```

Now, take a look at this snippet. Since the return value depends on an external variable, the function `count()` does not return the same value even if the same argument is passed for each function call. This function is unpredictable.

```javascript
let counter = 0;
const count = num => counter + num;

counter++;
console.log(count(5)); // 6
counter++;
console.log(count(5)); // 7
counter++;
console.log(count(5)); // 8
```

### Side effects

A function is said to have a side effect when

* the function call causes the mutation(change) of a non-local variable or state.
    
* the function body has input/output statements (examples: `prompt()`, `console.log()`).
    
* the function body has a statement that calls another impure function.
    
* function arguments are modified.
    

A good analogy for side effects is the world of Medicine. A tablet or pill for a disease like the common cold would help assuage the cold but might come with sleepiness or heaviness as a side effect.

Here's a modified version of the `count()` function from earlier. The counter update happens inside the function this time. Since the function call modifies an external variable, it has a side effect.

```javascript
let counter = 0;
const count = num => ++counter + num;

console.log(count(5))
console.log(count(5))
console.log(count(5))
```

Here's another example.

```javascript
// a function that adds a new key-value pair to an object
function addPropertyToObject(object, key, value) {
    object[key] = value;
    console.log(object)
}

const person = {
    name: 'Sunny',
    role: 'Developer'
}

addPropertyToObject(person, 'age', 25);
/* output:
{
    age: 25,
    name: 'Sunny',
    role: 'Developer'
}
*/
```

The function `addPropertyToObject` is mutating the `person` object by adding a new key-value pair, and also has a `console.log()` statement. So, it has side effects.

## Built-In Examples

All built-in methods in JavaScript are not pure. There are both pure and impure functions. Here are a few examples -

Array methods like `push()` `pop()` `sort()` and `splice()` are considered impure because, behind the scenes, they modify the original array that's passed to the methods as arguments.

On the other hand, `filter()` , `map()` , `slice()` and `reduce()` do not modify the original array, and is hence considered pure on the condition that their respective callback functions are also pure.

Another great example of an impure function would be `Math.random()` whose output cannot be predicted during the method call.

## Pure or Impure - What's the way to go?

Pure functions have a few solid advantages over impure functions

1. They are easy to read, test, and debug.
    
2. Another advantage is caching. Since they always return the same output for a given input, they can be cached or memoized to improve performance. This can be particularly useful for functions that are computationally expensive or called frequently.
    
3. Since they have no side effects, it's easier to modularize and make them reusable.
    

Although writing pure functions are considered a great practice, sometimes we need some impurity to get things done. Cases involving read/write operations, handling user input, and randomization are a few examples where the functions we write become impure by nature. So, the correct answer is, like in most cases, *it depends*!

## Recap

* Functions can be classified into pure and impure functions.
    
* Pure functions are predictable and have no side effects.
    
* Built-in methods in JavaScript are a mix of both.
    
* Writing pure functions is generally recommended, but some situations require impure functions.
    

Do spread the word if you got to learn something new today. It would mean the world to me! Have any questions or points to add? Comment away or drop me a DM on [Twitter](https://twitter.com/abin_john98). I'd love to connect :)

## Resources

### Detailed reads

1. [Syncfusion blog post](https://www.syncfusion.com/blogs/post/pure-and-impure-functions-in-javascript-a-complete-guide.aspx)
    
2. [Scaler blog post](https://www.scaler.com/topics/pure-function-in-javascript/)
    
3. [BecomeBetterProgrammer blog post](https://www.becomebetterprogrammer.com/javascript-pure-and-impure-functions/)
    
4. [Beta React Docs - keeping components pure](https://beta.reactjs.org/learn/keeping-components-pure)
    

### Standalone reads

1. [Wikipedia definition of Pure Function](https://en.wikipedia.org/wiki/Pure_function)
    
2. [Pure and impure functions interview questions](https://javascript.plainenglish.io/what-are-pure-functions-in-javascript-easy-topic-with-complex-questions-in-interview-aa4da06a4236)
    
3. [What is a side effect](https://softwareengineering.stackexchange.com/questions/40297/what-is-a-side-effect)