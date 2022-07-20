## Prototypal Inheritance via Constructor Functions

> Disclaimer: These notes were prepared when taking Jonas Schmedtmann's amazing [JavaScript course on Udemy.](https://www.udemy.com/course/the-complete-javascript-course/)
The code examples are from the course, the explanation has some pointers taken from the lecture, but is mostly my own understanding after spending some time on the topic, and the images used are all original. Please do correct me if I get something wrong anywhere :)

## Prerequisites
A sound understanding of the listed topics - 
1.  [Constructor funtions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor)
2. [The new operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new)
2. [The call method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)

## Setting the stage

Here's a constructor function `Person` that accepts two arguments, `firstName` and `birthYear`, and assigns them to its instance. There's also a method on its prototype property called `calcAge()`
```
const Person = function(firstName, birthYear){
	this.firstName = firstName;
	this.birthYear = birthYear;
}

Person.prototype.calcAge = function(){ console.log(2022 - this.birthYear) }
```
Here's another constructor function `Student` that accepts three arguments - `firstName`, `birthName`, and `course`. There's also a method on its prototype property called `introduce()`
```
const Student = function(firstName, birthYear, course){
	this.firstName = firstName;
	this.birthYear = birthYear;
	this.course = course;
}

Student.prototype.introduce = function(){ console.log(`My name is ${this.firtName}. I'm studying ${this.course}`) }
```

## Why take the Inheritance route here?
In a general sense, a student is also a person, and hence has `firstName` and `birthYear` properties. In the code above, assignments of these two properties in the `Student` function are duplicates of what we've seen in the `Person` function. 

* Inheritance helps remove duplicate code. We can make use of the `Person` constructor function within the `Student` function, i.e. call the `Person` function within the `Student` function so that the assignment of `firstName` and `birthYear` is taken care of in the former. We'll also be able to access the methods defined on 'Person' without having to duplicate them.
* Inheritance also helps be future-proof. If the definition of a "person" is changed, all we have to do is modify the `Person` function. If `Student` inherits from `Person`, the changes will be reflected automatically without having to change  its definition.

## In practice:
```
const Student = function(firstName, birthYear, course){
	Person.call(this, firstName, birthName)
	this.course = course;
}

const steve = new Student('Steve', 2000, 'CSE');
```
**Why is the `call()` method used? ** 

We are making a general function call without the `new` keyword, and general functions don’t have a `this` value. The call method helps to manually set what `this` will point to in the `Person` function body. We can pass the current working object as `this` to the function so that any operations involving `this` will reflect in the working object.

In our case, the reference of the instance of `Student` that is created when `new Student()` is called (i.e. `this`) is passed to `Person` where `firstName` and `birthYear` are assigned. `course` is assigned next within the `Student` constructor function.
![successful-object-creation.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658156366982/692ybjvOM.png align="center")

### Our first problem:
By the definition of prototypal inheritance, since `Student` now inherits from `Person`,  instances of `Student` should have access to methods defined on `Person.prototype` object. Currently, this is not the case. Since we just called `Person` from within `Student` without using the `new` keyword, the `Person.prototype` object wasn’t linked to `Student`

![first-challenge.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658157007323/Jf_MgvGoT.png align="center")

`.introduce()`, which is a method on the `Student.prototype` object, works correctly, but `calcAge()`, which is a method on the `Person.prototype` object doesn’t.

What needs to be done is set the prototype chain correctly such that instances of `Student` will have access to methods defined on `Person.prototype` object.

### Setting up the correct prototype chain
![current-proto-chain.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658242189958/Zmt8eeH6-.png align="left")

**we need to set the prototype of the `Student.prototype` object to point to the `Person.prototype` object.**


![required-proto-chain.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658242203027/pXMH9-7A5.png align="left")

We make use of `Object.create()` for the same
```
const Student = function (firstName, birthYear, course) {
  Person.call(this, firstName, birthYear);
  this.course = course;
};

//Linking Prototype
Student.prototype = Object.create(Person.prototype)

Student.prototype.introduce = function () {
  console.log(`My name is ${this.firstName}. I'm studying ${this.course}`);
};

const steve = new Student('Steve', 2000, 'CSE');
```
> It is important to implement the prototype chaining before adding methods to `Student.prototype` because after linking, `Student.prototype` will be an empty object whose prototype will be the `Person.prototype` object. Methods specified on `Student.prototype` before the link is implemented will be wiped out 👇

### Our second problem: incorrect constructor
Here’s the `Student.prototype` object **before** linking `Person.prototype` to it.
![second-challenge-p1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658158001890/JuyDJrgrf.png align="left")
Here’s the `Student.prototype` object **after** linking `Person.prototype` to it.
![second-challenge-p2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658158029288/SvTBpavBQ.png align="left")

The original `Student.prototype` object itself was replaced by the object that was returned by the `Object.create()` method we used to link the prototype. So the `introduce()` and `constructor()` properties were wiped out.

Just like we added `introduce()` to the new `Student.prototype` object, we have to manually set the constructor property on it. Otherwise, whenever we access the constructor property, the constructor property of  `Person.prototype` will be returned because of prototype chaining. This is not the expected behavior.

```
Student.prototype.constructor = Student;
```

![constructor-added.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658158350700/B2AXE1uh0.png align="left")

## Review
Let's review the steps involved in implementing prototypal inheritance.
1. Create parent and child constructor functions and add the parent's methods to its prototype object.
2. Call the parent constructor function from within the child constructor function if required.
3. Use `Object.create()` to link the parent's prototype object to the prototype of the child's prototype object.
4. Add the child's methods to the modified prototype object.
5. Set the constructor for the child.

### Final code
```
// Parent constructor function
const Person = function (firstName, birthYear) {
  this.firstName = firstName;
  this.birthYear = birthYear;
};

Person.prototype.calcAge = function () {
  console.log(new Date().getFullYear() - this.birthYear);
};

// Child constructor function
const Student = function (firstName, birthYear, course) {
  Person.call(this, firstName, birthYear);
  this.course = course;
};

// Linking Prototype
Student.prototype = Object.create(Person.prototype);

// Defining properties on the prototype object
Student.prototype.introduce = function () {
  console.log(`My name is ${this.firstName}. I'm studying ${this.course}`);
};
Student.prototype.constructor = Student;

// Creating an instance of type Student
const steve = new Student('Steve', 2000, 'CSE');

```
