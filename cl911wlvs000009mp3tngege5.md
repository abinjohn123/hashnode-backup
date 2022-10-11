# Event Propagation in JavaScript

## Introduction

Here's an exercise. In the embedded code snippet below, there are three nested blocks that are color-coded. Click on the different colored blocks. The output area below the blocks will display a message that says which block you clicked on. 

Click on the yellow block first, then green, and finally, blue.

<iframe src="https://codesandbox.io/embed/event-bubbling-cr7vdo?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="event-bubbling"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

You probably noticed something strange. When you click on a nested block, the output first displays that you clicked on that particular block but then proceeds to say that you clicked on the outer blocks too.

Why is this so?

That's what this article attempts to explain. We'll learn about one of JavaScript's features called event propagation, and what 'bubbling' and 'capturing' means.

## Events and Event Object

Events in JavaScript are one of the ways in which we interact with the DOM. Events can be triggered by the user (click, hover, page scroll), by the browser (transitionend, DOMContentLoaded), or even programmatically.

For each event, the browser creates an event object that contains details about the event, like the type of event, where the event was triggered, and more.

However, an event doesn't just start and end on the DOM element that triggered it. Meaning, if one were to click on a button, the click event doesn't just start and end on the button, but there are actually three phases for it.

They are - 
1. Capture Phase
2. Target Phase
3. Bubbling Phase

That's the order of the event phases, but we'll discuss Target -> Bubbling -> Capture, for easier understanding.

## Target Phase

Assume that you have a button on which you have attached a listener listening for click events on the button. When you do click on the button, the event handler on the listener is called. This execution of the handler on the element that originally triggered the event is called the Target Phase.

## Bubbling Phase

You clicked the button mentioned above, and that created the click event. However, the event doesn't just end there. The event runs the handlers on the target element (the button in our case), but then moves up to its parent, runs the handlers on the parents if any, moves up to its parent, runs the handlers there, and then the process continues till the root of the HTML document is reached.

In the [code snippet](https://codesandbox.io/s/event-bubbling-cr7vdo?file=/src/index.js) at the beginning of the article, I attached three event listeners, one each on the three nested blocks to track which block was clicked.

When clicked on the blue block, a click event occurs on it. The click event listener attached to the block executes its handler function, and then the event propagates to its parent i.e. the green block. The click event listener attached to the green block is executed after which the event propagates up to its parent, the yellow block. This process continues till the root of the DOM, the `window` object, is reached. Since I don't have any listeners added on `body`, `document`, or `window` objects, nothing else happens after executing the handler of the yellow block.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665298401632/r0I8OBdW0.png align="center")

The event starts from the innermost element that triggered it and then bubbles up the DOM tree. Hence the name Bubbling Phase.

### Stopping Bubbling

Bubbling is the default behavior in JavaScript, but it is possible to prevent bubbling anywhere in its path. `event.stopPropagation()` can be used to prevent further bubbling from the element on which the method is called.

## Revisiting the Event Object - target and currentTarget

The event object gives us information about 'target', the element that originally triggered the event, and `currentTarget`, the element on which the handler is currently being executed.

View the console logs in the code below. I've logged the `target` and `currentTarget` properties of the event object in the yellow block's event handler. That's why `currentTarget` is always yellow, irrespective of the block I click on.

<iframe src="https://codesandbox.io/embed/event-bubbling-cr7vdo?expanddevtools=1&fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="event-bubbling"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

## Capture Phase

An easy way to think about the capture phase is as the reverse of the bubbling phase, i.e. the event propagates down from the root to the target. The capture phase is rarely used and is hence turned off by default. We can only have either the capture or bubbling phase active.

To activate capturing, we pass a third argument `true` or `{capture: true}` to the `addEventListener` method. The event listener then will no longer listen to bubbling events for that handler.
  ```
  button.addEventListener('click', clickHandler, true)
  ```

## Summary

Events traverse or propagate up and down the DOM tree, which is the reason for the name 'Event Propagation'. Event propagation makes it possible to implement event delegation, which is a technique of adding one listener to a parent element, instead of adding listeners to multiple child elements. It's a neat trick to handle listeners effectively, but more on that in the next article.

## Additional Resources
1. [Detailed look into event propagation](https://javascript.info/bubbling-and-capturing)
2. [The Dangers of Stopping Event Propagation](https://css-tricks.com/dangers-stopping-event-propagation/)
3. [Event and Event Objects MDN](https://developer.mozilla.org/en-US/docs/Web/API/Event)
4. [More about event objects](https://medium.com/launch-school/javascript-lets-talk-about-events-572ecce968d0)
4. [My code demo](https://codesandbox.io/s/event-bubbling-cr7vdo?file=/src/index.js)