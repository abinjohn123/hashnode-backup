# Three Reasons Why JavaScript forEach is Special

The other day, I found an interesting code snippet in the 'Spot The Bug' section of a [Bytes newsletter](https://bytes.dev/). Can you spot it?

```javascript
const getCapitalizedInitials = (name) =>
  name
    .trim()
    .split(" ")
    .forEach((name) => name.charAt(0))
    .join("")
    .toUpperCase()
```

Since this article is about the `forEach` method, there's no surprise that the bug has something to do with the usage of the method. But what is it? Keep reading on to find out!

Here are three things that make the forEach method in JavaScript special compared to the other array methods and loops that the language offers.

## Return Value

Out of [all the iteration methods](https://www.w3schools.com/js/js_array_iteration.asp) available on Arrays, `forEach` is the only one that doesn't have a return value. It iterates over each element and executes a callback function for each iteration, but the method itself returns `undefined` at the end of the iteration.

This is unlike the other methods where a single value (`find`, `findIndex`, `some`) or an array itself (`map`, `filter`,`slice`) is returned. Because the return value is `undefined`, `forEach` cannot be chained with other array methods. That is the bug in the code snippet above. In our case, `forEach` should be replaced with `map`.

## Jump Statements

Since there is no return value, it's natural to approach `forEach` as an alternative to loops (for, while, for-of, for-in etc). I like to think of the method as a for-loop on steroids, but there is a difference here too. Jump statements (`break` and `continue`) have no effect inside the forEach callback. This is because, unlike regular loops, `forEach` doesn't have a loop body. It has a callback function instead that is called for each iteration. Callback functions are just like regular functions and don't have jump statements.

There are a few ways to mimic those behaviors if they are absolutely necessary. A `return` statement can be used inside the callback to mimic the `continue` statement. However, there isn't an easy way to stop or `break` the iteration. The only way is by throwing an exception. This is not a recommended practice. If early termination is necessary, other looping statements, like for-of or for-in can be used.

```javascript
const numbers = [1, 2, 3, 4 , 5, 6, 7, 8, 9, 10];


// regular loop that skips iteration for all even numbers
// and breaks it when 7 is reached
for(let i = 0; i < 9; ++i) {
    if(i % 2 === 0)
        continue;
    
    if(i == 7)
        break;
        
    console.log(i);
}
/* output: 
1
3
5
*/


// forEach method that tried to mimic the same behavior
numbers.forEach(num => {
    if(num % 2 === 0)
        continue; // error
    
    if(num === 7)
        return; // mimics the break statement
        
    console.log(num)
})
```

## Promises

`forEach` is not designed to work with Promises and asynchronous code in general. For the code snippet below, we might expect the `await` statement to work as it normally does, but it doesn't. `forEach` does not wait for each Promise to resolve, so all of them are executed in parallel.

Because of this, the iteration finishes before all the Promises are resolved, and the code execution continues. This is why 'end forEach' is logged before the console.log statements inside the callback function.

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

const wait = function(time) {
  return new Promise(resolve => setTimeout(resolve, time * 1000))
};

(async function(){
  console.log('start forEach');

  await numbers.forEach(async(num) => {
    await wait(num);
    console.log(num)
  });
  
  console.log('end forEach')
})();

/* output:
start forEach
end forEach
1
2
3
4
5
6
*/
```

If you need to work with async code, it is recommended to use for-of or the regular for loop that supports asynchronous JavaScript.

```javascript
 (async function(){
  console.log('start for-of');
  
  for(const num of numbers) {
    await wait(num);
    console.log(num)
  }
  
  console.log('end for-of')
})();

/*
output: 
start for-of
1
2
3
4
5
6
end for-of
*/
```

## Recap

* `forEach` doesn't have a return value, and hence cannot be chained with other array methods.
    
* Jump statements, like `continue` and `break` do not work inside the callback function.
    
* `forEach` is not designed to work with asynchronous JavaScript.
    

## Further Reading

1. A three-part series on fundamental array methods - [map](https://abinjohn.in/understanding-map), [filter](https://abinjohn.in/understanding-filter-map-and-reduce-13), [reduce](https://abinjohn.in/understanding-reduce)
    
2. A great read about [not using forEach with async-await](https://gist.github.com/joeytwiddle/37d2085425c049629b80956d3c618971)
    
3. [Handling Promises in parallel](https://abinjohn.in/parallel-promises)
    
4. [Three for loops in JavaScript](https://abinjohn.in/three-for-loops)
    
5. [forEach MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)