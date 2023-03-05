---
title: "The Reduce Method - Understanding Filter, Map, and Reduce (3/3)"
datePublished: Sat Aug 27 2022 14:02:22 GMT+0000 (Coordinated Universal Time)
cuid: cl7bz29wr086caznv1u4gaaok
slug: understanding-reduce
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1661609119608/c7pQIvL__.png
tags: javascript, web-development, beginners, learn-coding, 4articles4weeks

---

> This is the final part of a three-part series. Check out [Part 1: The filter method](https://abinjohn.in/understanding-filter-map-and-reduce-13) and [Part 2: The map method](https://abinjohn.in/understanding-map)

## The Reduce Method

Congratulations on making it this far! We've covered two powerful array methods and are now ready to conquer the final boss - the Reduce method.

The reduce method is quite different from the Filter and Map methods. Both Filter and Map had specific return values. i.e. The Filter method would return an array of elements that passed the filter condition, and the Map method would return an array of elements that were returned by its callback function for each iteration.

The Reduce method, however, doesn't have a specific return value. We, the developer, can set up the method to return what we want. It can return a number, a string, an array, an object - anything! This flexibility is what makes it so powerful.

The method is usually used to 'reduce' an input array to a single value. An example would be using the method to calculate the sum of the array elements. We're inputting an array of elements, and we are reducing the array to a single value - the sum. Hence the name.

Here's the basic structure:

```javascript
//Syntax
array.reduce(callbackFunction, initialValue)

// Common usage
const reducedValue = array.reduce(function(accumulator, currentElement, indexOfElement, fullArray){
// function body
}, initialValue)
```

Let's break it down:

## The Callback Function

The callback function has four parameters

1. An accumulator (also referred to as 'previous value')
    
2. The current array element
    
3. The index of the current element
    
4. The entire array
    

The accumulator stores the value that is returned by the callback function during each iteration. That way, the value returned in one iteration can be accessed in the subsequent iteration through the accumulator. Ultimately, the value returned by the callback function in the last iteration is what the Reduce method returns. That was a mouthful! But it will become clear as we check out a few examples.

## Optional Initial Value Argument

The `initialValue` argument is an optional argument that is used to set the initial value of the accumulator variable.

If `initialValue` is not specified, the method starts iteration with `accumulator = array[0]` and `currentElement = array[1]`. The iteration starts at index 1 in this case.

If `initialValue` is specified, the method starts iteration with `accumulator = initialValue` and `currentElement = array[0]`. The iteration starts at index 0 in this case.

**Values for the first iteration**:

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661586706549/wTqXY5k9q.png align="center")

## Examples

1. Sum of an array: Summing an array is one of the easiest implementations of the reduce method. Here's how it goes:
    

```javascript
const array = [9, 7, 11, 5, 8];

const sum = array.reduce((accumulator, element) => {
  const returnValue = accumulator + element;
  return returnValue;
})
console.log(sum) // 40
```

Since the accumulator stores the value returned from the callback function of one iteration and makes it available to the next one, we keep on adding the elements to the accumulator for each iteration.

Here's a visualization of the different variable values of the callback function for every iteration. Note that `initialValue` was not passed, so accumulator starts with the value at index 0.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661607727904/T8sEZUsdu.png align="left")

1. Finding the maximum value in an array
    

```javascript
const array = [9, 7, 11, 5, 8]

const max = array.reduce((acc, el) => el > acc ? el : acc)
console.log(max) // 11
```

Let's break down the callback function.

* For the first iteration, the value of the accumulator `acc` is 9 and `el` is 7
    
* For each iteration, the callback function checks `el` against `acc`
    
* If `el > acc`, `el` is returned, which updates the value of `acc` as `el`
    
* If `el < acc`, `acc` itself is returned so that the value of `acc` doesn't change.
    

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661607757554/TocQhg4I5.png align="center")

## Returning is important!

A mistake that developers make is assuming that just updating the value of the accumulator inside the callback function is enough for the value to be available in the next iteration.

Here's an example -

```javascript
const numbers = [100, 200, 300, 400, 500];

const sum = numbers.reduce(function(acc, el) {
    console.log(acc, el);
    acc += el;
})
console.log(sum);

/*
100 200
undefined 300
undefined 400
undefined 500
undefined
*/
```

For the first iteration, `acc` is initialized with `numbers[0]` and `el` is `numbers[1]`. Even though `acc` is updated inside the callback function, the function does not return any value. Remember - the value returned by the callback function for one iteration is the value of the accumulator in the next iteration. Since the callback doesn't return anything, the value of `acc` for the second iteration is `undefined`. Same for the subsequent iterations.

## Taking it further

Reduce can also be used to implement the filter and map methods as well. That's how flexible it is.

```javascript
const array = [9, 7, 11, 5]

// filter method
// number should be greater than 7 to pass the filter
const filteredArray = array.reduce((acc, el) => {
  if (el > 7)
    acc.push(el);
  return acc;
}, []);

// map method
// returns an array with the squares of the array elements
const mappedArray = array.reduce((acc, el) => {
  acc.push(el ** 2);
  return acc;
}, []);

console.log(filteredArray) // [9, 11]
console.log(mappedArray) // [81, 49, 121, 25]
```

In both the implementations shown above, we've used the `initialValue` argument to set the initial value of the accumulator to that of an empty array. This is because the final values we need from the filter and map implementations are arrays. By initializing the accumulator with an empty array, we can then push elements to it and then return the array at the end.

> Have an idea of what the final value returned by the method should be and set the initial value and the return statement inside the callback accordingly.

**The filter method:**

Elements that pass the filter condition are pushed into the accumulator array.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661607972084/X-HUsErk0.png align="center")

**The map method:**

The squared value for each array element is pushed into the accumulator array.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661608262800/6MI0qSH5-.png align="center")

Here's an example that returns an object. Counting occurrences of elements:

```javascript
const fruits = ['Mango', 'Banana', 'Orange', 'Apple', 'Orange', 'Banana']

const occurances = fruits.reduce((acc, el) => {
  // if element is present inside the object, increase its count
  // else initialize count to 1
  if (acc[el])
    acc[el]++;
  else acc[el] = 1;

  return acc;
}, {})

console.log(occurances)
/*
{
  Apple: 1,
  Banana: 2,
  Mango: 1,
  Orange: 2
}
*/
```

For this example, we set `initialValue` as an empty object, because we want the method to return an object that gives the number of occurrences for the array items.

For each iteration, the function checks if the current element is already a key of the accumulator object. If yes, the corresponding value is increased by one. If not, a new key is created and its value is set to one. This object is then returned.

---

As a challenge, try implementing a function that uses the reduce method to perform both filtering and mapping. The function takes in an array of numbers and returns a new array that contains the squares of just the odd numbers inside the original array.

## Conclusion

The reduce method is the swiss army knife of the array methods that can seemingly do anything. It's a handy tool when tackling complex data manipulation that wouldn't be possible with the other methods.

That wraps up the Reduce method, and this series as well. We've covered the three major array methods - Filter, Map, and Reduce, and I hope that by now you really do understand them :)

Drop a comment, a Twitter DM, or an email to ask questions or discuss anything JavaScript!