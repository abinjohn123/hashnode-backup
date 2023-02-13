# Introduction to JavaScript Spread Syntax & Rest Parameter

The Spread syntax and Rest parameter are two powerful concepts that were introduced to the JavaScript language with ES6 in 2015. At first glance, the two look similar, and developers who are just discovering the three dots often have a hard time figuring them out (I know I definitely did!)

Whenever we see `...` in a code snippet, it is either spread or rest. So, what's the difference? and how are they useful?

## The Spread Syntax

An *iterable* in JavaScript is an entity that can be looped over. An array is a good example. The spread syntax takes an iterable and "spreads" out its individual elements as comma-separated values.

Imagine we have an array called `evenNums` that contains a few even numbers and an array `oddNums` that contains a few odd numbers. We are tasked with creating a new array `allNums` that has all the numbers in `oddNums` followed by all the numbers in `evenNums`.

Here's the "traditional" way to do that -

```javascript
const evenNums = [2, 4, 6];
const oddNums = [1, 3, 5];

const allNums = [oddNums[0], oddNums[1], oddNums[2], evenNums[0], evenNums[1], evenNums[2]];
```

Here's what we are actually doing -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676191055892/fdd57262-1100-44d5-b770-76f87da3f5e1.png)

We are listing out each item of the array and separating them by commas. To create the `allNums` array, we do that for the `oddNums` array, add a comma, and then do the same for `evenNums` array.

Listing out the individual array elements separated by commas is exactly what the spread syntax does. Using the spread syntax, the code now becomes -

```javascript
const allNums = [ ...oddNums, ...evenNums];
```

Let's now revisit the definition: The spread syntax **spreads** out the iterable's contents and separates them by commas.

But what if we did this instead?

```javascript
const allNums = [oddNums, evenNums]
```

In this case, we are taking the two arrays and adding them as elements of `allNums`. We are not picking the individual elements. Here's what `allNums` would look like if we took the approach above.

```javascript
const allNums = [oddNums, evenNums]

console.log(allNums) //    [[1, 3, 5], [2, 4, 6]]
```

### Use cases

1. Merge or concatenate arrays
    
    This is what we did in the example above. The spread operator is an easy and simple way to merge multiple arrays into one.
    
2. Convert a string to an array
    
    A string is also an iterable. So by using the spread syntax, it can be easily converted to an array
    
    ```javascript
    const string = 'JavaScript';
    
    const arrayFromString = [ ...string ];
    
    console.log(arrayFromString)
    // ["J", "a", "v", "a", "S", "c", "r", "i", "p", "t"]
    ```
    
3. Create copies
    
    The spread syntax is a great way to create shallow copies of an iterable. Because of the way in which JavaScript handles different value types, simply assigning an array or object to a new value doesn't create a true copy \[More on this [here](https://abinjohn.in/object-mutability)\]. The spread syntax helps to spread the original array contents and put them in a new array.
    
    ```javascript
    const baseArray = [1, 2, 3, 4];
    
    // Simply assigning baseArray to sameReference doesn't create a copy.
    // They both point to the same location in memory.
    // Any change made to either will reflect in the other.
    const sameReference = baseArray;
    
    
    // shallowCopy contains the same items as baseArray, 
    // but they don't point to the same memory location.
    // Any change made to either will not reflect in the other.
    const shallowCopy = [ ...baseArray ]
    
    baseArray.push(100)
    
    console.log(baseArray) //[1, 2, 3, 4, 100]
    console.log(sameReference) //[1, 2, 3, 4, 100]
    console.log(shallowCopy) //[1, 2, 3, 4]
    ```
    
    ---
    

## The Rest Parameter

If you've understood the spread syntax, Rest is easy (pun intended). Rest is the opposite of Spread i.e it takes comma-separated values and turns them into an array. This might be confusing at first, so let's quickly jump into an example.

Imagine we are [destructring](https://abinjohn.in/destructruing-javascript) an array as shown below.

```javascript
const [first, second, ...others] = [1, 2, 3, 4, 5, 6, 7, 8]
```

Here's what logging the three variables to the console looks like.

```javascript
console.log(first) // 1 
console.log(second) // 2
console.log(others) // [3, 4, 5, 6, 7, 8]
```

Here's what happens behind the scenes -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676200334372/1516cc7d-7bdb-4d7c-ac7b-77be6fdeb4dc.png)

> Both Rest and Spread use the same syntax: the three dots (`...` )
> 
> When the dots are used on an array, it spreads the array to its individual elements. It is called the spread syntax in such cases.
> 
> When the dots are used on a comma-separated list, it packs the list items into an array. It is called the rest pattern in such cases.

The rest pattern is popular with destructuring assignments like the one shown above, to pack the unwanted elements into an array. A more popular usage of rest is with function parameters. This is where the name 'Rest parameter' comes from as well.

### Rest in function parameters

Here's a function that takes two numbers and prints their sum

```javascript
 function printSum(num1, num2) {
    console.log(num1 + num2)
}

printSum(3, 5) // 8
```

What if we wanted to make a more generalized function that can take any number of inputs and then print their sum? That's where rest comes in.

When we call a function with more than one argument, we pass the arguments as a comma-separated list within the function call's parenthesis.

```javascript
printSum(1, 3, 4, 100);
```

It is this comma-separated list that is passed to the function parameters. We can make use of the rest pattern to pack this comma-separated list into a single array.

As a first step, let's use the rest pattern to pack the items into an array and then log it to the console.

```javascript
function printSum(...operands) {
    console.log(operands)
}

printSum(1, 3, 4 ,100) // [1, 3, 4, 100]
```

Notice how the comma-separated list has now become an array

We can now use a loop to iterate over the `operands` array and print the sum. A [for-of loop](https://abinjohn.in/three-for-loops) is used in the example below.

```javascript
function printSum(...operands) {
  let sum = 0;
  for (const num of operands)
    sum += num;
  console.log(sum);
}

printSum(1, 3, 4, 100) // 108
```

Such a function parameter that uses the spread syntax to pack incoming comma-separated items into a single array is called the rest parameter.

The rest parameter can be used in combination with regular parameters as well, but we have to be careful with two things

1. The rest parameter should be the last parameter in the parameter list
    
    ```javascript
    // INCORRECT
    function someFunction (a, ...rest, b) {
     //
    }
    
    // CORRECT 
    function someFunction (a, b, ...rest) {
     //
    }
    ```
    
2. There should only be one rest parameter for a given function
    
    ```javascript
    // INCORRECT
    function someFunction (a, ...rest, ...someMore) {
     //
    }
    
    // CORRECT 
    function someFunction (a, ...rest) {
     //
    }
    ```
    

Here's one final example. This function takes a greeting and a list of names, and prints out the greeting followed by the name for each name

```javascript
function printGreeting(greeting, ...names) {
    for(const name of names)
        console.log(greeting + ', ' + name)
}

printGreeting("Hello", "John", "Doe", "Smith");
/*
Hello, John
Hello, Doe
Hello, Smith
*/
```

## Recap

* Whenever we see `...` in JavaScript, it is either spread or rest, depending on the usage.
    
* When used on an array, it spreads the array elements into a comma-separated list.
    
* When used on a comma-separated list, it packs all the items into an array.
    
* The spread syntax is commonly used to merge arrays and make copies
    
* Rest is commonly used in destructuring and also as a rest parameter in functions that need to accept an indefinite number of arguments.
    

I hope you got to learn something new today. If you did, do consider sharing this post. Got any questions? Comment away or connect with me on [Twitter](https://twitter.com/abin_john98)!
