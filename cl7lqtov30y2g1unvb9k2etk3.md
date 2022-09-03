## Dynamic Object Keys in JavaScript

Imagine you're tasked to create a function that creates and returns an object literal from the keys and values that are fed into it as its arguments.

At first go, you might be tempted to go with something like this:
```js
function objectMaker(keys, values) {
  const object = {};
  
  // guard clause to exit the function
  // if the number of keys and values don't match
  if (keys.length !== values.length)
    return false;
  
  // creating key-value pairs in the object
  keys.forEach((key, i) => object.key = values[i])

  console.log(object)
}

objectMaker(['name', 'age', 'id'], ['Abin', 24, '123456'])
```

The output of that code, however, would look like this
```
{
  key: "123456"
}
```

There are two obvious concerns with the output
1. There is only one key-value pair
2. "key" is not a key value that we had in the array that we passed to the function

## The problem
Object items can be accessed in JavaScript in [two ways](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_accessors)
* Dot notation
* Bracket notation

So for a simple `person` object shown below, its properties can be accessed via any of the notations
```js
const person = {
	firstName: 'Abin',
    age: 24,
    id: 123456
}

// dot notation
console.log(person.firstName) // Abin

// bracket notation
console.log(person['firstName']) // Abin
```

The problem with our code arises because of a small catch associated with using the dot notation, which is that **whatever we specify after the dot becomes the final property name.**

So even though we intended ` keys.forEach((key, i) => object.key = values[i])` to create multiple key-value pairs with `object.key` referring to a new key for each iteration, `object.key` always accesses the same property `key` in the object.

This can be demonstrated by logging the object in each iteration
```js
// modifying the forEach function to log the object
// at the end of each iteration
  keys.forEach((key, i) => {
    object.key = values[i];
    console.log(`Iteration ${i+1}:`)
    console.log(object)
  })

```
The above code gives us this output:
```
"Iteration 1:"
{
  key: "Abin"
}
"Iteration 2:"
{
  key: 24
}
"Iteration 3:"
{
  key: "123456"
}
```
For the first iteration, since the object is empty at this point, a new key called 'key' is created when the function tries to access `object.key` and assign a value to it.

For all subsequent iterations `object.key` is then reassigned to the new value.

This is why in the original code, the final `console.log(object)` statement gave us an object with a key of 'key' and a value that was the final element of the values parameter.

## The solution
The solution to our problem is quite simple - the bracket notation. 

Unlike dot notation, where what came after the dot became the final property name of the object, bracket notation offers more flexibility as we can mention expressions inside them.

This makes it possible to create and access keys dynamically. Here's an example:
```
const person = {
	firstName: 'Abin',
    age: 24,
    id: 123456
}

const key = 'Name'
console.log(person['first' + key]) //Abin
``` 
The above code snippet works because `'first' + key` evaluates to 'firstName`.

We can use the same technique for our problem to implement dynamic keys. Here's the code:
```
function objectMaker(keys, values) {
  const object = {};

  if (keys.length !== values.length)
    return false;

  keys.forEach((key, i) => object[key] = values[i])

  console.log(object)
}

objectMaker(['name', 'age', 'id'], ['Abin', 24, '123456'])
```

And the output:
```
{
  age: 24,
  id: "123456",
  name: "Abin"
}
```

## Taking it a step further - Computed Property Names
Up until ES6, one drawback of the bracket notation was that it couldn't be used directly in object literals at the time of declaring them. 
This meant that there had to be one line of code that declared the object and another to access a property via the bracket notation.
```
// Before ES6:
// one line of code to declare the object
// and another to access property via bracket notation

const key = 'firstName'

const obj = {};
obj[key] = 'Abin'
```

The computed property name feature of ES6 allows declaring an object literal with dynamic property names. The result of an expression enclosed in brackets becomes the property name.

The code snippet above can be re-written as follows
```
const key = 'firstName'

const obj = {
  [key]: 'Abin'
}
```
## Conclusion
That wraps up this short article on dynamic object keys. Let me know if you've encountered a scenario where you needed dynamic keys. Feel free to drop a comment or leave a message on [Twitter](https://twitter.com/abin_john98).