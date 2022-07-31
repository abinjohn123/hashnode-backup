## Template Functions Using Partial Application

## Introduction
Partial Application is the technique of a function returning another function, with one or more of its arguments *locked* or *preset*. They are a great way to make templates for functions that have many parameters.

Let's make that a bit clear with a slightly exaggerated example.

Imagine you're tasked with implementing functions that create DOM elements. You take the `insertAdjacentHTML` route that needs three properties to work with - 
  1. A parent element
  2. A typeModifier that specifies where the created DOM element will be placed with respect to the parent
  3. The HTML string that is to be rendered as the DOM element.

```
function createElement(parent, position, html) {
  parent.insertAdjacentHTML(position, html)
}

```
You now have a clean, general function that can be used to create a DOM element anywhere on the page. But let's say there are multiple points in your code where you need to add an element as the last child of a list element. Every time you need to add this element, you have to call the `createElement` with the first two arguments always the same.

```
const listParent = document.querySelector('.list-parent');
const aRandomParent = document.querySelector('.random-parent');
//
//
//
createElement(listParent, 'beforeend', '<li>First Call</li>')
//
//
createElement(aRandomParent, 'afterEnd', '<p>Another call of the function<p>'
//
//
createElement(listParent, 'beforeend', '<li>Second Call</li>')
//
//
//
createElement(listParent, 'beforeend', '<li>Third Call</li>')

```
This is quite repetitive and if the typeModifier is ever to be changed, all the places where `beforeend` is specified will have to be updated. 

Wouldn't it be nice if we could create a template of the `createElement` function with the parent and typeModifier preset? We wouldn't have to write lengthy function call statements, and modifying the call would be as simple as modifying the original template. Sounds good, right?
That's where partial applications come in.

## Implementing Partial Application
Let's first modify the  `createElement` function a bit to make it return another function.
```
function createElement(parent, position) {
  return function(html){
  	parent.insertAdjacentHTML(position, html)
  }
}
```
`createElement` now returns another function with the parent and typeModifier parameters locked, but accepts an argument for the HTML string. We can assign this to a variable which can then be called later.

```
const listEndAppend = createElement(listParent, 'beforeend')
```

`listEndAppend`  can now be called by passing the required HTML string and the string will be appended to the listParent.

> The reason why the child function has access to the outer function's parameters at a later time, well after its execution has finished, is because of [closure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

```
listEndAppend('<li>First Call</li>')
//
//
//
//
listEndAppend('<li>Second Call</li>')
```

To make general calls, first, call the `createElement` function with the required arguments and then immediately call the child function by passing its required arguments.
```
createElement(aRandomParent, 'afterEnd')( '<p>Another call of the function<p>')
```

## Using the `Bind` method.
Unlike the call or apply methods, bind doesn't immediately invoke the function, but returns a copy with all the parameters applied. 

> "returns a copy with all the parameters applied."
  Sounds like a template, right?

The `createElement` function remains the same as the original version
```
function createElement(parent, position, html) {
  	parent.insertAdjacentHTML(position, html)
}

```
The function call is where the difference comes in. We use the bind method to preset the parent and position values. Since we are not working with `this` in the function, we pass the first argument as `null`.
```
const listEndAppend = createElement.bind(null, parentEl, 'beforeend');
```
We can then call `listEndAppend` with the third value that the function needs i.e. the HTML string.
```
listEndAppend('<li>Second item</li>');
```

I personally like to use the bind method to implement partial applications because the original function doesn't have to be modified. The function can be used like any other function and called as required.
```
//To create a template
const listEndAppend = createElement.bind(null, parentEl, 'beforeend');
listEndAppend('<li>Second item</li>');
listEndAppend('<li>Third item</li>');
listEndAppend('<li>Fourth item</li>');

//For general calls
createElement(aRandomParent, 'afterEnd', '<p>Another call of the function<p>')
```
