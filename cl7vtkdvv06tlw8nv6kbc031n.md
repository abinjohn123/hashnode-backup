## Declarative Vs Imperative Programming - My Understanding

The other day I built my first React app. It was a thrilling experience as it had been something that I'd been keeping for later assuming it was way too complicated, but the instructor did a stellar job of keeping things simple and fun.

Before we started to get our hands dirty and code, he recommended taking a look at the [React](https://reactjs.org/) website and understanding what the three key highlights of react given on the homepage meant.

They were-
1. Declarative
2. Component-Based
3. Learn Once, Write Anywhere

This article is my "layman's understanding" of what the first item on the list, i.e. Declarative, means. I spent some time looking into the topic so that you hopefully won't have to :)

## Declarative vs Imperative
Declarative programming and imperative programming are just two different programming paradigms. i.e. different approaches to programming to get something done.

In imperative programming, we are concerned with how exactly what we want gets done. In declarative programming, we aren't concerned with how things get done. We just want to get the end result.

Let's say you and a friend are going on a road trip from place A to B.
In the imperative programming approach, you'd be giving out all the routes, turns, and stops to take to get to point B. In the declarative programming approach, you'd let your friend know where the final destination is and then kick back and relax, while they figure out the route themselves.

## Code example
Here is a vanilla JS code that updates a counter value and displays it on the DOM each type a button is clicked. Slide the bar on the left to view the code.

<iframe src="https://codesandbox.io/embed/jolly-moore-hry6q8?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="jolly-moore-hry6q8"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

Here's the flow of code:
> User clicks on a button -> the counter value is updated -> updated value is displayed on the DOM in multiple places

In Vanilla JS, each step of the process has to be coded manually.
We need to set up selectors to let our script know the places where the counter value needs to be updated, and write code that then updates the DOM with the new counter value.

```
// Selecting elements where the output must be displayed
const counterOutputs = [...document.querySelectorAll(".counter-value")];

// Updating the value to the DOM
function updateDOMCounter(value) {
  counterOutputs.forEach((outputField) => (outputField.innerText = value));
}
```
This is an example of imperative programming. We are involved with the nitty-gritty of our implementation.

Here's the same example in React
<iframe src="https://codesandbox.io/embed/counter-update-react-forked-lxuy7m?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="counter-update-react (forked)"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

We use a state hook for the counter, which gives a variable `counter` that has the counter value and a `setCounter` setter function that can be used to update the counter value.

To update the counter value on the DOM, all we have to do is mention `{ counter }` wherever we need the counter value to be displayed and call `setCounter(newValue)` within the listeners of the buttons to update the value.

React takes care of updating the `counter` value **and** rendering the updated value on the DOM. We don't have to bother with writing explicit code to select the DOM elements and update the counter value.

This is an example of declarative programming. We just tell React that we need the updated value of `counter` on the DOM wherever it is displayed. React takes care of the rest.

## Conclusion
This sums up the basic differentiation between imperative and declarative programming. There is no right or wrong approach here. The imperative approach is relatively a bit tedious, but gives low-level control over implementation, while the declarative approach is quicker and simpler, with a sacrifice on control.

## Resources 
Here are a few resources that I went through while trying to understand the topic.
1. [An amazing, detailed Dev.to article](https://dev.to/ruizb/declarative-vs-imperative-4a7l)
2. [A good read, with multiple metaphors](https://ui.dev/imperative-vs-declarative-programming)
3. [Another simple, fun read](https://codeburst.io/declarative-vs-imperative-programming-a8a7c93d9ad2)
4. [Great starter for React](https://www.youtube.com/watch?v=KUJsaM-hAjs)