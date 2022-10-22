# Event Delegation in JS

## Introduction
In my article on [event propagation](https://abinjohn.in/js-event-propagation), we saw how an event bubbles up the DOM tree, triggering all the listeners listening to it on the way.

Event delegation is a neat technique for handling events that makes use of event propagation, especially bubbling, for its implementation.

Imagine there are many elements that should listen to a click event. The idea of event delegation is that instead of adding click event listeners to each element, we attach a single listener to a container element and catch the event from there.

## Establishing The Need

Here's a practical example: embedded below is the dashboard menu bar for a banking application. It currently has three menu items: Notifications, Recent Transactions, and Account Summary. They are an unordered list, with the parent container being the `ul` tag with the class `dashboard-items`. Click and drag the vertical bar on the left to view the code.

<iframe src="https://codesandbox.io/embed/event-delegation-c0dtfc?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="event-delegation"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

So far so good. The concern begins when we start to add event listeners to the items. Each menu item should be clickable and perform the appropriate action when clicked.

The common approach would be to attach separate listeners to each menu item and call the appropriate handlers. There are two problems with that.
1. As the number of menu items goes up, the number of listeners will also go up. More listeners imply more memory requirements, and that could affect performance.
2. For a more dynamic application like a todo-list, we'd have to implement code to attach listeners to each new task entry. A usual todo-app would have buttons on each task entry to mark the task as complete and delete the task, so that means adding two listeners for each task entry. As the complexity of the application goes up, the complexity of the code will also go up.

Event delegation solves these two problems quite efficiently. Here's how.

## Implementation of Event Delegation

In the code snippet above, we have to add a click listener to each of the list items. To implement event delegation, we need - 
1. A container element: This can be the parent or another ancestor element. This is the element that contains all the elements that we need to attach the listeners to. Because of [event bubbling](https://abinjohn.in/js-event-propagation), any event triggered on those elements would then bubble up to this container element and we'll then catch and process the event from there.
2. The `target` property on the event object: Each event has an event object, which has the `target` property. This property lets us know the element on which the event was triggered.

We attach the appropriate listener to the container element and then access `event.target` in the handler to know which element triggered the event.

In my code demo, I added a custom attribute `data-option` to each of the list items. In the snippet below, I try to access the target element and then log its `data-option` attribute by using the `getAttribute` method.
```
const menuContainer = document.querySelector(".dashboard-items");

function menuClickHandler(event) {
  const menuItem = event.target;
  console.log(menuItem.getAttribute("data-option"));
}

menuContainer.addEventListener("click", menuClickHandler);
```

Open the console of the sandbox below and try it out
<iframe src="https://codesandbox.io/embed/event-delegation-c0dtfc?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="event-delegation"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

You might notice something strange. If you click on the item icon or the text, the console prints out `null`. If you click on the space around the icon or text, the console prints out the correct `data-option` attribute. Why would this be?

This is because `event.target` gives us the exact element that triggered the event. Each list entry has two children - an icon and text. The list entry is the one that has the custom attribute, not the children. So when the children are clicked, `event.target` points to them, and their `data-options` value is null.

To work around this, we implement code such that even if we click on the children, we'll travel up the DOM, till we reach the list entry `li` element that contains them. We make use of the `closest` method for that.

```
const menuContainer = document.querySelector(".dashboard-items");

function menuClickHandler(event) {
  const target = event.target;
  const menuItem = target.closest(".dash-item");
  console.log(menuItem.getAttribute("data-option"));
}

menuContainer.addEventListener("click", menuClickHandler);

```

`closest` will traverse up the DOM from the element that's called on, till it finds a match to the query that we pass in. In our case, the list elements have the class `dash-item` and so we pass in `.dash-item` as the query.

Here's the modified snippet in action
<iframe src="https://codesandbox.io/embed/event-delegation-forked-um4xyo?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="event-delegation (forked)"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

That's it! We have successfully implemented event delegation. Now that we know which element triggered the event, we can add logic in the handler to control the flow code. If-else, switch-case, it's all up to you!

Since the listener is on the container `ul` element, list entries can be added or removed without having to worry about handling the listeners in multiple places in the code. There's only one listener and so only one place to work on.

## Summary

Event delegation is truly a powerful tool to handle events efficiently. It makes code maintenance easy and is great for performance.

I implemented event delegation in a todo-list project of mine. You can check it out [here](https://github.com/abinjohn123/todoom-app)

Feel free to connect with me on [twitter](https://twitter.com/abin_john98). I'd love to chat!


