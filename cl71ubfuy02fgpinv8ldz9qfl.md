---
title: "The Map Method - Understanding Filter, Map, and Reduce (2/3)"
datePublished: Sat Aug 20 2022 11:51:50 GMT+0000 (Coordinated Universal Time)
cuid: cl71ubfuy02fgpinv8ldz9qfl
slug: understanding-map
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1660995961188/X9rZWg5P-.png
tags: javascript, web-development, javascript-framework, education

---

> This is Part 2 of a three-part series. [Check out Part 1: The filter method](https://abinjohn.in/understanding-filter-map-and-reduce-13)

## Recap

We learned about the filter method in the last section. Here's a quick summary of how the filter method works

1. The method iterates over each array element
    
2. A callback function is called for each iteration. This function can take up to three parameters: the array element, the index of the element, and the entire array.
    
3. If the function returns a truthy value, the element passes the filter and is pushed to a new array.
    
4. This resulting new array that contains only the elements that pass the filter is returned when the method has finished iterating over all the array elements.
    

## The Map Method

Now we move on to the map method. Its working closely resembles that of the filter method, with the difference being the nature of the callback function.

Here's how the map method works

1. The method iterates over each array element
    
2. A callback function is called for each iteration
    
3. The **value returned by the callback function** is pushed to a new array
    
4. This new array is returned at the end when the method has finished iterating over all the array elements.
    

The filter method gave us an array containing the elements that passed the filter condition. The map method gives us an array containing the return values of the callback function. i.e. Each element of the array is mapped to a new value. Hence the name.

Here's the basic structure

```javascript
const mappedArray = array.map(function(currentElement, indexOfElement, fullArray) {
  //function body
})
```

You can think of the map method like the conveyor belt shown below. It performs the same action on the incoming items, but the result varies depending on the input.

<iframe src="https://giphy.com/embed/kIsxboXxVimBi" width="480" height="271" class="giphy-embed"></iframe>

[via GIPHY](https://giphy.com/gifs/salih-heart-valentines-day-art-kIsxboXxVimBi)

## The callback function

Like the filter method, the callback function for the map method has three parameters

1. The current array element
    
2. Index of the current element
    
3. The entire array
    

The value returned by this callback function is pushed to a new array and this array is finally returned by the method when it has finished iterating over all the elements.

## Map Examples

1. Let's start with a simple example of doubling an array's values.
    

```javascript
const array = [1, 2, 3, 4];

const mapped = array.map(function(el){
    return el * 2;
})

console.log(mapped) //[2, 4, 6, 8]
```

We took each element, multiplied it by 2, and then returned the resulting value.

We can replace the callback function with its arrow counterpart to condense the code further

```javascript
const mapped = array.map(el => el * 2)
```

1. Here's an example that also makes use of the index parameter of the callback function
    

```javascript
let array = [2, 5, 1, 0, 4 ,7];

array = array.map((num, i) => num * i);

console.log(array) // [0, 5, 2, 0, 16, 35]
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660995494989/hIAN_qdL7.png align="left")

1. Map is a handy tool when you want to perform the same action on different data. I was recently building a tool where I had to represent time (hours, minutes, and seconds) in two digits. So an input of `12:5:7` had to be corrected to `12:05:07` i.e. if the value was less than 10, add a zero before it.
    

```javascript
[hours, minutes, seconds] = [hours, minutes, seconds].map(
    time => String(time).padStart(2, '0') //Add 0 at the start if time is less than 10
  );
```

The code snippet maps over the `[hours, minutes, seconds]` array, adds a zero at the start if required, and then returns the resulting array which I reassign to the original `hours`, `minutes`, and `seconds` variables via array destructuring.

1. Map is also quite useful when you want to extract data from complex or nested objects. Imagine you have an array of objects, with each object containing employee information. You can use `map` to easily extract information from these objects
    

```js
const empDetails = [{
    firstName: 'Abin',
    lastName: 'John',
    email: 'ajohn@example.com'

  },
  {
    firstName: 'John',
    lastName: 'Doe',
    email: 'jdoe@example.com'
  },
  {
    firstName: 'Rocky',
    lastName: 'Bhai',
    email: 'salam@rockymail.com'
  },
  {
    firstName: 'Tony',
    lastName: 'Stark',
    email: 'ironman@starkindustries.com'
  }
]

const fullNames = empDetails.map(employee => {
  return employee.firstName + ' ' + employee.lastName;
})

console.log(fullNames)
// ["Abin John", "John Doe", "Rocky Bhai", "Tony Stark"]
```

## Conclusion

That sums up the map method. By now you would have a good idea of how the callback functions work for the filter and map methods. Up next is the reduce method - the final boss of our mini-series. It is a versatile and powerful tool, whose callback function can be modified to get the result that we want. More on that in the final part of the series.

> [Read Part 3 (Reduce method)](https://abinjohn.in/understanding-reduce)