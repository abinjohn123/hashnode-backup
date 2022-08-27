## Understanding Filter, Map, and Reduce (3/3)

> This is the final part of a three-part series.
    [Check out Part 1: The filter method](https://abinjohn.in/understanding-filter-map-and-reduce-13)
  [Check out Part 2: The map method](https://abinjohn.in/understanding-map)

## The Reduce Method
Congratulations on making it this far! You've learned two powerful array methods and are now ready to conquer the final boss, the reduce method.

The reduce method is quite different from the filter and map methods. Both those methods had a specific return value. i.e. The filter method would return an array of elements that passed the filter condition, and the map method would return an array of elements that were returned by its callback function on each call. 

The reduce method, however, doesn't have a specific return value. We, the developer, can set up the method to return what we want. It can return a number, a string, an array, an object - anything! This flexibility is what makes it so powerful.

The method is usually used to 'reduce' an input array to a single value. An example would be using the method to calculate the sum of the elements in an array. We're inputting an array of elements, and we are reducing the array to the sum, which is a single value. Hence the name.

Here's the basic structure:
```
const reducedValue = array.reduce(function(accumulator, currentElement, indexOfElement, fullArray){
// function body
}, initialValue)
```

Let's breakdown the method

## The Callback Function
The callback function takes in four arguments
1. An accumulator (also referred to as 'previous value')
2. The current array element
3. The index of the current element
4. The entire array

The accumulator is a variable that stores the value returned by the callback function for each iteration. The value stored in one iteration is accessible in the next iteration. It is the value of the accumulator at the end of the iteration that's returned by the method.

## Optional Initial Value Argument
The `initialValue` argument is an optional argument that is used to set the initial value of the accumulator variable.

If `initialValue` is not specified, the method starts iteration with `accumulator = array[0]` and `currentElement = array[1]`
If `initialValue` is specified, the method starts iteration with `accumulator = initialValue` and `currentElement = array[0]`

**Values for first iteration**: ![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661586706549/wTqXY5k9q.png align="center")

## Examples
Just the breakdown alone is not sufficient to solidify one's understanding of the method. Let's go through some examples.

1. Sum of an array: 
Summing an array is one of the easiest implementations of the reduce method. Here's how it goes:

  ```
  const array = [9, 7, 11, 5, 8];

  const sum = array.reduce((accumulator, element)=>{
	  const returnValue = accumulator + element;
	  return returnValue;
  })
  console.log(sum) // 40
  ```
  Since the accumulator stores the value returned from the callback function of one iteration and makes it available to the next one, we keep on adding the elements to the accumulator for each iteration.

  Here's a representation of all the variable values of the callback function for each iteration
  
  ![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661607727904/T8sEZUsdu.png align="left")

2. Finding the maximum value in an array

  ```
  const array = [9, 7, 11, 5, 8]

  const max = array.reduce((acc, el) => el > acc ? el : acc)
  console.log(max) // 11
  ```
  Let's break down the callback function.
  - For the first iteration, the value of accumulator `acc` is 9 and `el` is 7
  -   For each iteration, the callback function checks `el` against `acc`
  -   If `el > acc`, `el` is returned, which updates the value of `acc` as `el`
  -   If `el < acc`, `acc` itself is returned, so that the value of `acc` doesn't change.
  
  ![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661607757554/TocQhg4I5.png align="center")

    > NOTE: It's mandatory to return a value from the callback function if it works with the accumulator. If not, acc will be `undefined` after the first iteration and that could cause issues.

3. Reduce can also be used to implement the filter and map methods as well. That's how flexible it is.

  ```
  const array = [9, 7, 11, 5]
  
  // filter method
  // number should be greater than 7 to pass the filter
  const filteredArray = array.reduce((acc, el) => {
    if (el > 7)
      acc.push(el)
    return acc
  }, []);

  // map method
  // returns an array with the squares of the array elements
  const mappedArray = array.reduce((acc, el) => {
    acc.push(el ** 2)
    return acc;
  }, []);

  console.log(filteredArray) // [9, 11]
  console.log(mappedArray) // [81, 49, 121, 25]
  ```
  In both the implementations shown above, we've used the `initialValue` argument to set the initial value of the accumulator to that of an empty array. This is because the final values we need from the filter and map implementations are arrays. By initializing the accumulator with an empty array, we can then push elements to it and then return the final array.

  **The filter method:**
  ![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661607972084/X-HUsErk0.png align="center")
  **The map method:**
  ![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661608262800/6MI0qSH5-.png align="center")

4. Counting occurrences of elements: 
  ```
  const fruits = [ 'Mango', 'Banana', 'Orange', 'Apple', 'Orange', 'Banana']
 
  const occurances = fruits.reduce((acc, el)=>{
	  if(acc[el])
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

## Conclusion
The reduce method is the swiss army knife of the array methods that can seemingly do anything. It's a handy tool when tackling complex data manipulation that wouldn't be possible with the other methods.

That wraps up the reduce method, and this series as well. We've covered the three major array methods - filter, map, and reduce, and I hope that by now you really do understand them :)

Drop a comment, a Twitter message, or an email to ask questions or discuss anything JavaScript!
