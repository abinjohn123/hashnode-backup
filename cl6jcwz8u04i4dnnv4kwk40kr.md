## Understanding Filter, Map, and Reduce  (1/3)

## Introduction
Filter, Map, and Reduce methods are powerhouses in JavaScript that offer great flexibility when working with arrays and make data manipulation quite easy. However, they can also be a bit difficult to wrap your head around, especially if you're just starting out with array methods. In this three-part series, I go through each of these methods and explain them with practical examples. We'll start with the easiest henchman- the filter method and work our way to reduce, the final boss.

## Setting The Stage
All three methods that we are about to explore share some similarities.
1. They don't affect or manipulate the original array.
1. They iterate over every element of the array on which they are called. That's how they work. That is also why they are preferred in appropriate situations compared to iterating over the array elements using a loop. There's less code required, they look clean, and the ability to whip off a neat reduce method is also a brag-worthy skill.
2. A callback function is executed for each element. All the magic happens in these callback functions. This is where we define the filter conditions, data manipulation, and so on. More on those later.
3. At the end, when the methods have finished iterating over all array elements, they return something. In the case of filter and map methods, the value returned is an array, while in the case of the reduce method, well, the return value is anything that you, the developer, want it to be!

Now let's start with the first method.


## The Filter Method
As the name suggests, this method is used to filter out contents from your array. The method iterates over each array element, runs a filter condition against it and pushes the element to a new array if it passes the condition. This new array that contains only the elements that passed the condition is returned by the method once it has finished iterating over all the elements.

Here's the basic structure
```
const filteredArray = array.filter(function(currentElement, indexOfElement, fullArray){
	//body
})
```

Think of the filter method as one of those height threshold signs next to rides in a theme park. A lot of people could be standing in the queue, but only the ones who meet the criteria go through.
![animals-rollercoaster-roller_coaster-fairground_ride-fair_ride-carnival-denn232_low.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1659807491545/RmEzH9HTh.jpg align="center")
[image source](https://www.cartoonstock.com/cartoon?searchID=CS435891)


### The callback function
This is where we define the checks or the condition that each element will be tested against.

The function has three parameters
1. The current array element
2. Index of the current element
3. The entire array

If the function returns a truthy value, the element passes the filter and is pushed to the new array. 

>Theoretically, the callback function body can contain anything, but if we want the filter to actually filter out something, we need to put some filter condition inside the function body and return true or false from it.


### Filter Examples

* Imagine you have an array containing money transfers made in your account. You want to see just the incoming transfers. A filter is a neat approach to achieve this.

```
const transfers = [100, 200, -50, -600, 200, 10000, -50, -345, 23, 190]

const incoming = transfers.filter(function(val) {
  return val > 0
})
console.log(incoming) // [100, 200, 200, 10000, 23, 190]
})
```

* Imagine you have an array of objects, each object containing the name and height of a person. You want to create a new array of people who satisfy the minimum height requirement of 160cm.

```
const peeps = [
{
	name: 'Alice',
  height: 162
},
{
	name: 'Bob',
  height: 155
},
{
	name: 'Carl',
  height: 150
},
{
	name: 'Dennis',
  height: 172
}
]

const tallPeeps = peeps.filter(function(person){
	return person.height > 160;
})

console.log(tallPeeps)
```
The output comes like this:
```
//console.log output
[{
  height: 162,
  name: "Alice"
}, {
  height: 172,
  name: "Dennis"
}]
```


* Here's another example

```
const numbers = [1, 2, 3, 4, 5]

const filtered = numbers.filter(function(val, i){
	console.log(`Value: ${val}, index: ${i}`)
	return true;
})

console.log(filtered)
```

What do you think the console output would be?

```
//Console output:
"Value: 1, index: 0"
"Value: 2, index: 1"
"Value: 3, index: 2"
"Value: 4, index: 3"
"Value: 5, index: 4"
[1, 2, 3, 4, 5]
```
Since we hardcoded the return value of the function as `true` every element of the array passed the filter condition.

* And finally, [here's a codewars problem](https://www.codewars.com/kata/52597aa56021e91c93000cb0/train/javascript) that asks you to take an array and move all of the zeros to the end, while preserving the order of the other elements.

i.e. for an input of `[1, 2, 0, 1, 0, 1, 0, 3, 0, 1]` the output should be `[1, 2, 1, 1, 3, 1, 0, 0, 0, 0]`

Filter makes that so easy! We can use one filter to filter out the non-zero numbers and another to filter out the zeros. We can then concatenate both resulting arrays and we have the solution ready!
```
function moveZeros(arr) {
  const nonZeros = arr.filter(function(val){
    return val !== 0
  });
  const zeros = arr.filter(function(val){
    return val === 0
  });
  const result = nonZeros.concat(zeros);
  return result;
}

//The same function using arrow functions
function moveZerosArr(arr) {
  const nonZeros = arr.filter(val => val !== 0);
  const zeros = arr.filter(val => val === 0);
  const result = nonZeros.concat(zeros);
  return result;
}
```
Super easy, right? Notice how using arrow functions condenses the function code block even further.

## Conclusion
Here's an illustration to help remember what the filter method does.
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659808555414/HLcI_aKm4.png align="left")

That sums up this introduction to the filter method. As said earlier, the filter method is pretty straightforward. Only the elements that pass a condition are copied to a new array, which is then returned at the end. We'll look into `map()` next. Stay tuned for that article!



