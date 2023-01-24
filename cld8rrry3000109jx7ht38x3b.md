# Object Mutability in JavaScript

JavaScript is a funny language. If we declare a primitive variable with `const` and try mutating(modifying) it, we'll get an error. Well, that's expected behavior. However, if we declare an array or an object with `const` and try modifying one of its properties, it works without any problem.

```javascript
const number = 23;
number = 32; // Uncaught TypeError: Assignment to constant variable.

const object = {
    firstName: 'Abin',
    role: 'developer',
}

object.firstName = 'Kevin' // works without a problem
```

Similarly, if we copy a primitive variable and make changes to the copied variable, the original variable remains unchanged. However, do the same for arrays or objects, and you'll see that any change made to the copy gets reflected in the original variable.

```javascript
const a = 23;
let b = a;
b = 52;
console.log({a, b}) // {a: 23, b: 52}

const developerA= {
    firstName: 'Abin',
    role: 'developer',
}
const developerB = developerA;
developerB.firstName = 'John';
console.log(developerA, developerB)
//{firstName: 'John', role: 'developer'} {firstName: 'John', role: 'developer'}
```

This post explains the magic behind JavaScript objects that make them behave as shown in the examples above. To do that, we have first to understand the basics of how memory works in JavaScript and what happens behind the scenes when we declare variables.

## How memory works

JavaScript stores variables in memory. Specifically, variables are stored in the computer's RAM (Random Access Memory) while the program is running. For the purposes of this post, let's think of memory as a chessboard. Each cell of the chessboard can store one value.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674291819548/1e255728-8fcf-44c3-8319-8f928bb3289f.png align="center")

Every time we declare a variable, one cell is occupied with the value that we pass in. Whenever we reference the variable in our code, JavaScript takes a look at the cell and takes the value from it.

But how does JavaScript know which cell to look at if we have five variables and want variable #3? It's not guaranteed that the cells get occupied in a sequence, so we can't just access the third cell from the start. Wouldn't it be nice if the cells had addresses or IDs so that we know which one to access for a particular variable?

In fact, this is exactly how things work. Each cell in the memory has an address. When we declare a variable, JavaScript takes a look at the memory, finds a free cell, stores the value in the cell, and provides us with the address of the cell. So when we say that a particular variable holds a value of `50`, what we actually mean is that the variable holds the address of the memory location that has `50` stored in it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674363562246/652719ad-e70b-4e61-b72a-0ac570ac4fe7.png align="center")

The reverse happens when we try to fetch the values later in our code.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674363598599/3e22f27d-6e9a-42be-a1c3-430cc49909f2.png align="center")

Now that we know what happens on the memory when we declare variables, let's take a closer look at how JavaScript treats different data types.

## Two data types - Primitive Vs Reference

Data types in JavaScript are categorized into two - primitive and reference. `number`, `string`, `boolean`, `undefined`, `null`, `bigint` and `symbol` are primitive data types. Arrays, objects, and functions are reference data types.

The difference primarily lies in where they are stored in memory. Remember our chessboard RAM? In JavaScript, the available memory is split into two parts. The first part is called the Call Stack, and the other is the Heap. Primitive data types are stored in the Call Stack while Reference data types are stored in the Heap, and it is this difference that makes objects and arrays behave the way they do with respect to mutability.

> The Call Stack, like the name suggests, is a stack data structure. A lot more happens on the call stack than just allocating memory for primitive data types, but that is beyond the scope of this post. You can read about the [Execution Context](https://www.freecodecamp.org/news/execution-context-how-javascript-works-behind-the-scenes) to learn more about what happens on the call stack.

Let's see what happens on the Call Stack and Heap when we declare variables of different types. Assume that the two bottom rows of our chessboard memory (a1 to h2) are allocated for the Call Stack and the two upper rows (a7 to h8) are for the Heap.

### Declaring primitive data types

We now know that -

1. A variable actually holds the address of the memory location that stores the value
    
2. Primitive data types are stored in the call stack
    

Combining both, we can see that when a primitive variable is declared in JavaScript, a memory location is assigned to it, and the value of the variable is stored at that location. Both the value and the memory location address are stored on the call stack. So for a declaration as shown below -

```javascript
const firstName = 'Abin'
```

Assuming that the value `Abin` is stored in cell c1, `firstName` will now point to c1.

### Declaring reference data types

This is where things get a bit interesting. When we declare a reference data type, the value itself is stored in the Heap, but the address of the memory location is stored in the Call Stack. The address of the cell in the Stack that holds the heap memory location is what the variable will point to. Confusing, right?

Imagine we create a `car` object as shown.

```javascript
const car = {
    name: 'Corolla',
    make: 'Toyota',
}
```

The value will be stored in the Heap memory. Let's assume the address of the location is d7. However, the variable `car` doesn't point to d7. A variable declaration always points to a location on the Call Stack. So, the address d7 is stored somewhere on the Call Stack and `car` then points to that location. Assuming d7 is stored at e1, `car` will point to e1, which in turn points to d7. That's how reference data types are stored.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674365704871/4b113d97-2159-4928-bf31-3db09eea5db3.png align="center")

## Data types and mutation

When we declare a variable using `const`, the variable name and the value stored in the associated memory location get fixed. The value stored at that particular location can no longer change. However, since the values stored in the Call Stack for primitive and reference data types are different, the mutability behaves differently for the two.

For primitive data types, the actual values are stored in the Call Stack. So when we try to mutate a primitive type, we are trying to change the value stored in the stack. This is against the expected behavior when using `const` and JavaScript throws the 'Assignment to a constant variable' error.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674369356564/489a2bae-4124-4aef-8256-eac2b9039ce7.png align="center")

For reference types, the actual values are stored on the Heap. The Call Stack only contains the address location of the heap. When we try to mutate a reference type, we are in turn trying to mutate the value stored on the Heap, which is not a problem. Modifying an array or the properties of an object doesn't change their location on the Heap, which ensures that the address referenced in the Call Stack doesn't change. This is the reason why we can modify objects and arrays declared using `const`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674369427244/c4639a51-c40c-4082-8d21-552b21ae68a8.png align="center")

## Copying Variables

We've covered variable declaration. Now we'll look into what happens when we copy variables and then mutate the copies.

When we copy a variable, the copied variable will also point to the same memory location as the original variable.

Consider this code snippet:

```javascript
const firstName = 'Abin';
let anotherName = firstName;
```

Assume that 'Abin' was stored in memory location c1. `firstName` then points to c1. When `firstName` is copied to `anotherName`, `anotherName` will also point to c1. One peculiar thing about primitives is that primitive values stored in the Call Stack cannot be mutated. If an existing variable that has a primitive value is reassigned to a new value, JavaScript assigns a new cell with the updated value and then points the variable to the new cell. It doesn't update the existing cell with the new value.

Therefore, when either of the variables is changed, JavaScript will allocate a new cell with the updated value and point the variable to that location. The unchanged variable hence points to the original location c1 and the changed variable now points to another location. That way, any mutation done to copied variables doesn't affect the original variables

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674472477936/ea8babb2-61d6-43fd-b371-2f46b488a6ea.png align="center")

The same principle applies to reference types as well, i.e. a copy will point to the same location on the Call Stack as the original variable. The difference here is that, unlike primitives, an update to the value happens on the Heap; not the Call Stack. Since the value stored on the Call Stack remains unchanged, the copy and the original variable will always point to the same memory location on the Heap. It is because of this reason that any update that happens on the copy gets reflected on the original - both point to the same object in the Heap.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674473683077/5a12521e-16eb-4ff3-a3e5-814e60bfb6b8.png align="center")

## Creating 'copies' of reference types

The trick to creating actual copies that won't affect the original variables is to create a new object and copy each value from the original.

  
An easy way to copy objects is the spread operator.

```javascript
const car = {
    name: 'Corolla',
    make: 'Toyota',
}

const newCar = { ...car }
```

There are a few more ways to create copies. [Here's a nice read](https://www.javascripttutorial.net/object/3-ways-to-copy-objects-in-javascript/) on different ways to copy objects.

There are a lot more options and flexibility for copying arrays since they are iterable. [This article](https://www.freecodecamp.org/news/how-to-clone-an-array-in-javascript-1d3183468f6a/) covers the various ways to copy arrays.

## Recap

* A variable doesn't directly hold the value assigned to it. Rather, it holds the address of the memory location that has the value stored in it.
    
* Data types are categorized into primitive and reference types. `number`, `string`, `boolean`, `undefined`, `null`, `bigint` and `symbol` are primitive data types. Arrays, objects, and functions are reference data types.
    
* Primitives are stored directly in the Call Stack. Reference types are stored on the Heap, and the memory location on the Heap is stored in the Call Stack.
    
* When variables are copied, both the original and copied variables will initially point to the same memory location in the Call Stack.
    

That was a long one. Thanks for making it this far! Do let me know if you got to learn something new today :)

## References

1. [MDN Primitives](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)
    
2. [What happens when variables are copied](https://www.zhenghao.io/posts/javascript-memory#string-interning)
    
3. [Memory Management](https://felixgerschau.com/javascript-memory-management/)