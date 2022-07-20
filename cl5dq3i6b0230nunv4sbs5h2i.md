## Use Event Listeners The Right Way!

## Introduction
Here's a normal event listener listening for a click event
```
element.addEventListener('click', function(e) {
  if(!e.target.classList.contains('active'))
    e.target.classList.add('active')
})
```
Just from reading that snippet, it is not easy to understand what exactly the listener function is doing. We can only infer that a class of `active` is added to the target element, but no additional context is obtained, like what adding that class does to the element.
Such functions that do not have a name are called Anonymous functions, and they pose two major problems - 
1. It is difficult to understand what the function does
2. The function block cannot be reused

Now take a look at the following code snippet
```
function toggleBackgroundColor(e) {
    if(!e.target.classList.contains('active'))
      e.target.classList.add('active')
}

element1.addEventListener('click', toggleBackgroundColor)
element2.addEventListener('click', toggleBackgroundColor)

```
Even if you don't know the exact code flow, from reading the event listener assignments it is at least clear that the click event does something related to toggling the background color of the element. Also, the same function can now be used by multiple listeners without having to duplicate the code.

Therefore, by defining listener functions outside of the event listener, we encourage code readability and code reusability in our program. This evidently makes testing and debugging easier as well.

## How to define listener functions outside of listeners
Now that we've established that it is a good practice to define listener functions outside of the event listeners, let's look at three ways of implementing the same.

### 1. The direct way
The easiest and most direct way to do it is to define the function separately and just mention the function name as the second parameter of the `addEventListener()` method.
Here's the previous snippet again:
```
function toggleBackgroundColor(e) {
    if(!e.target.classList.contains('active'))
      e.target.classList.add('active')
}

element1.addEventListener('click', toggleBackgroundColor)
element2.addEventListener('click', toggleBackgroundColor)

```
The event object is automatically passed by Javascript in this case. There's just one drawback to this method which is that we cannot pass any parameters to the function. The only parameter that gets passed by default is the `event` object. The following two methods enable us to pass other parameters as well.

### 2. Via an anonymous function
A workaround to pass parameters to listener functions is to use an anonymous function in the event listener that then calls the listener function. In the below example, we pass both the event object and an additional parameter `color`
```
function switchBackgroundColor(event, color) {
	event.target.style.backgroundColor = color;
}

element.addEventListener('mouseover', function(event){ switchBackgroundColor(event, 'pink')})
element.addEventListener('mouseout', function(event){ switchBackgroundColor(event, 'blue')})
```

### 3. The bind method
The bind method on a function returns a new function that has its `this` associated with the first argument of the method. Unlike the call or apply methods, bind doesn't immediately invoke the function, but returns a copy with all the parameters applied. We use this feature to our advantage to get a copy of the listener function with all our parameters set.
```
function switchBackgroundColor(color) {
  this.style.backgroundColor = color;
}
element.addEventListener('mouseover', switchBackgroundColor.bind(element, 'pink'))
element.addEventListener('mouseout', switchBackgroundColor.bind(element, 'blue'))
```
## Conclusion
Depending on your purpose, any of the above methods can be used to keep the event listener and the listener function separate. The important thing **is** to have the element selector, the event listener, and the listener function separate so that you have clean code that's easy to follow through.
