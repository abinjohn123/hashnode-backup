# Three For Loops in JS

Loops are an integral part of any programming language as they allow us to condense what could be ten or a hundred lines of code into a few. JavaScript offers multiple loops with each having its own space and use case. This article introduces and explains three loops that have the `for` keyword in them.

## For Loop

The for loop is one of the most basic loops in JavaScript. Here's the syntax
```
for(let i = 0; i < 10; ++i){
  console.log('this is a for loop');
}

```

There are three key parts to a for loop
1. Initialize: this expression initializes variables that will be used to control the loop. In the code snippet above, a variable `i` is defined with the value 0

2. Check: this expression is what controls the loop. This expression is evaluated before each iteration **(including the first one)**. If it returns `true`, the statements inside the loop body execute. If it returns `false`, the loop immediately terminates.

  In our code, the condition `i < 10` returns true until `i`'s value is 10. When `i` is 10,  the expression returns false and the loop terminates without executing the body.

3. Update: this expression is evaluated after each iteration, and is used to update the control variables. In our code, `i` is incremented by 1 after each iteration. If `i` was not updated, its value would always be 0, the check would always return true, and the loop would never terminate. This is called an infinite loop.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665842006215/1ISY6VtE8.png align="center")

Here's the code flow
1. execute the initialization expression
2. check the condition
3. If the condition returns true, execute the loop body. Else, go to step 6
4. after the loop body has finished execution, execute the update expression
5. Go back to step 2
6. exit the loop

## For-Of Loop

The for-of loop is used to iterate over iterable objects, like arrays, strings, node lists, etc. Unlike the for loop, for-of doesn't have control variables or loop conditions in place to decide when to exit the loop and is ideal in places where iteration over the entire object is required.

```
  const alphabets = ['a', 'b', 'c', 'd'];

  for (const alphabet of alphabets) {
    console.log(alphabet);
  }
```

A simple way to think about the loop would be "**for** each item **of** the object, do what's prescribed in the loop body"

The for-of loop has two key parts
1. Variable
2. Iterable object

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665843384541/S_qJd7Uni.png align="center")

The loop iterates over each item of the object, and for each iteration, the value of  `variable` will be that particular item over which the loop is iterating over.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665843510500/jb8ots0M2.png align="center")

## For-In Loop

The for-of loop made it possible to iterate over arrays and strings; two popular data types in JavaScript. So what about object literals and those created via the `new` operator? That's where the for-in loop comes into the picture.

The for-in loop iterates over the enumerable properties of an object. All the object properties created by simple assignment or inheritance from another object are enumerable by default, so in most cases, the for-in loop iterates over all the properties of an object.

```
const randomObject = {
  1: 'this is one',
  firstName: 'John',
  alphabet: 'e',
  2: 'this is two'
}

for (const key in randomObject) {
  console.log(`${key}: ${randomObject[key]}`)
}

/* output
"1: this is one"
"2: this is two"
"firstName: John"
"alphabet: e"
*/
```

The for-in loop has two key parts
1. Key
2. Object

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665896800865/Hq_b63lne.png align="center")


For each iteration, the value of  `key` will be a key from the object in string format. Notice how in the code sample above, the order in which the properties were accessed was different from which they were listed in the object declaration. [Here's an interesting read](https://dev.to/frehner/the-order-of-js-object-keys-458d) that shines some light on this behavior.

## Conclusion

That wraps up three of the for loops in JavaScript. There is another loop - the for await-of, but that's out of the scope of this article. I have also omitted for-each from this article as it is technically an array method and not a loop, although it iterates over the array elements.

Feel free to connect with me on [twitter](https://twitter.com/abin_john98). I'd love to chat!


