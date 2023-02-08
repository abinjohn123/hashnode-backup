# Array & Object Destructuring in JavaScript

## Introduction

Destructuring is a JavaScript technique to extract values from arrays and objects quickly. Traditionally, we use indices to get values from arrays and properties (keys) to get values from objects. With destructuring, it is possible to extract multiple values in one go, thereby removing the need to access properties or elements by individual keys or indices.

## Array Destructuring

Here's a code snippet that assigns array elements to different variables. Notice how extensive the code is because we're accessing the elements one by one.

```javascript
const numbers = [1, 2, 3];

const a = numbers[0];
const b = numbers[1];
const c = numbers[2];

console.log(a, b, c) //1 2 3
```

Here's the same example using destructuring. The code becomes concise and clean.

```javascript
const numbers = [1, 2, 3];

// destructuring
const [a, b, c] = numbers;

console.log(a, b, c); //1 2 3
```

Here's how the destructuring statement works -

* We wrap the variables that we want to assign values to in a pair of brackets.
    
* We now have an array on the left and right sides of the destructuring statement.
    
* Each variable on the left gets assigned a value corresponding to its index from the right
    
    i.e. `a` has an index of 0, so a gets the value `numbers[0]` . Similarly, `b` is assigned `numbers[1]` and `c` is assigned `numbers[2]`
    

A simpler way to think about destructuring is as shown.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675776093752/5b22ea9e-c816-4cd4-a06d-2d5686101ed8.png align="center")

## Object Destructuring

We made use of array indices for array destructuring. Here, we'll use object property names. And instead of wrapping the variables in brackets, we wrap them in a pair of braces.

```javascript
const person = {
    name: 'Abin',
    language: 'JS',
};

const { name, language } = person;

console.log(name, language) // Abin JS
```

The destructuring statement checks for the property on the left in the object specified on the right and then assigns the value.

## Additional Features

Now that we've covered the basic syntaxes for array and object destructuring, we can move on to a few more 'features' that the syntax brings.

### Default values

It is possible to define default values for the destructured variables on the left. This is particularly useful when we're destructuring an object whose content cannot be determined beforehand (An API response, for example)

```javascript
const numbers = [1, 2, 3];
const [a, b, c, d = 10] = numbers;

console.log(a, b, c, d); // 1 2 3 10
```

```javascript
const person = {
    name: 'Abin',
    language: 'JS'
};

const { name, language, country = 'India' } = person;
console.log(name, language, country) // Abin JS India
```

### Skipping values

Blank spaces can be used to skip values when destructuring arrays. This is useful to ignore values that we are not interested in from the array being destructured.

```javascript
const [, a, b, , c] = [1, 2, 3, 4, 5];
console.log(a, b, c) // 2 3 5
```

### Overriding property names

It is possible to override the property names when destructuring objects.

```javascript
const shop= {
    name: 'Kiki Stores',
    city: 'JS Town',
    category: 'Jewelry'
};

const { name: shopName, category, city: shopCity } = shop;
```

The above snippet copies `shop.name` to `shopName` and `shop.city` to `shopCity`

### Using the rest operator

When just a few values are required from an object, it is a common practice to use the rest operator to unpack all the unwanted values to a single array.

```javascript
const [a, b, ...rest] = [1, 2, 3, 4, 5, 6, 7, 8, 9]

console.log(a, b , rest) // 1 2 [3, 4, 5, 6, 7, 8, 9]
```

> The rest operator should only be used with the last element
> 
> ```javascript
> const [...rest, b, c] // INCORRECT
> 
> const [a, b, ...rest] // CORRECT
> ```

### Nested objects

When unpacking nested arrays or objects, it is important to maintain the same nesting structure on the left-hand side.

```javascript
// try guessing what the output would be
// a combination of skipping values and nesting has been used here
const [a, , [b, ], [, [c,d]]] = [2, 4, [5, 6], [7, [8, 9]]]
console.log(a, b, c, d) // output?
```

```javascript
const library = {
  name: 'Crossword',
  location: 'Lulu Mall',
  timings: {
    mon: {
      open: '0900',
      close: '1900'
    },
    tue: {
      open: '0900',
      close: '1900'
    },
    wed: {
      open: '0900',
      close: '1900'
    },
    thu: {
      open: '0900',
      close: '1900'
    },
    fri: {
      open: '0900',
      close: '1900'
    },
    sat: {
      open: '0900',
      close: '2100'
    },
    sun: {
      open: '0900',
      close: '2100'
    }
  }
}

// copying Sunday timings as 'sundayTimings'
const {
  timings: {
    sun: sundayTimings
  }
} = library;

console.log(sundayTimings)
/* output:

{
  close: "2100",
  open: "0900"
}

*/
```

## Destructuring Use Cases

1. Swapping values
    
    Destructuring makes it possible to swap values between two variables without using a temporary third variable.
    
    ```javascript
    let a = 23, b = 45;
    console.log(a, b) // 23 45
    
    [a, b] = [b, a];
    
    console.log(a, b) // 45 23
    ```
    
2. Return multiple values from a function and destructure at the receiving end
    
    ```javascript
    function someFunction() {
        .
        .
        .
        return {value1, value2, value3}
    }
    
    const { value1, value2, value3 } = someFunction();
    ```
    
3. For functions with a lot of parameters, it could get difficult to keep track of the order when passing the values during the function call. In such scenarios, passing an object as the argument and destructuring it inside the function removes the need to maintain the order.
    
    ```javascript
    function printInfo({name, language, location}) {
        //
    }
    
    printInfo({
        location: 'India'
        name: 'Abin',
        language: 'JS'
    })
    ```
    
4. React hooks make use of destructuring to define variables and setter functions. In the `useState` example below, the hook call returns an array that contains the state variable and its setter function. They are destructured and assigned to the variables that we define.
    
    ```javascript
    const [count, setCount] = useState(0);
    ```
    

## Recap

* Destructuring is a JavaScript technique used to quickly extract values from Arrays and Objects.
    
* It removes the need to access elements or properties individually.
    
* Destructuring simplifies the handling of multiple values in function calls and return statements.
    

## Resources

1. [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
    
2. [**Jonas Schmedtmann's stellar JavaScript course**](https://www.udemy.com/course/the-complete-javascript-course/)