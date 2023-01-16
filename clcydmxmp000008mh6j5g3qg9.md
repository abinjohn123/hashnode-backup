# Building the Same App: Vanilla JS vs React

It's January 13th as I write this post, and it has been almost 8 days since I started doubling down on React as part of my "developer resolutions" for 2023. I [put off learning React](https://abinjohn.in/2022-wrapped#heading-i-havent-react-ed-yet) for over a year because of insecurities about not knowing *enough* JavaScript, but I'm changing that this year.

I've been learning from the [Beta React Docs](https://beta.reactjs.org/) because (a) It's more up-to-date and has removed class-based components, and (b) it has interactive code snippets and chapter exercises. After I got a brief understanding of [State](https://beta.reactjs.org/learn/state-a-components-memory) and how it works, I decided to build one of the examples in the docs using both React and Vanilla JS to compare the developer experience. This post is a log of my observations.

## The App

The App is a simple carousel that displays information about sculptures. The info is obtained from an array, and there are two buttons to cycle through the list. Here's a working preview:

<iframe src="https://codesandbox.io/embed/gallery-working-o01qhm?fontsize=14&hidenavigation=1&theme=dark&view=preview" style="width:100%;height:500px;border:0;border-radius:4px;overflow:hidden" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

## Observations

### Setting up the editor

Creating the project and setting up the files seems to be much simpler in vanilla JS - there are just two files after all. For React, there are a couple of different ways to get started as mentioned on this [installation page](https://beta.reactjs.org/learn/installation), and anything other than the CDN links method seem to be a bit complicated for someone just starting out.

If using online IDEs, like [CodeSandBox](https://codesandbox.io/) or [CodePen](https://codepen.io/), the experience is pretty much the same as the platforms take care of most of the setup. All the developers will have to do are minor tweaks to their liking.

### Linking HTML and JavaScript

This is one area where React shines for me. With regular HTML and JavaScript, the two are separate files that are linked via the script tag. Although the two are linked, because of the [imperative nature](https://abinjohn.in/dec-vs-imp), we have to still go about selecting the DOM elements that we want to interact with and also write code for any update that may happen to them. This results in a lot of `.querySelector()` or `getElementById()` selections, `addEventListener()` methods, and update functions.

Here's the code in Vanilla JS to select all the HTML elements that will undergo an update and to attach event listeners to the buttons.

```javascript
// selectors
const btnPrev = document.getElementById("btn-prev");
const btnNext = document.getElementById("btn-next");
const btnDetails = document.getElementById("btn-details");
const currentScp = document.getElementById("current-art");
const scpName = document.getElementById("heading--item-name");
const scpArtistName = document.getElementById("heading--artist-name");
const scpDetails = document.getElementById("sculpture-details");
const scpImage = document.getElementById("sculpture-image");

// event listeners
btnNext.addEventListener("click", handleNextClick);
btnPrev.addEventListener("click", handlePreviousClick);
btnDetails.addEventListener("click", handleDetailsClick);
```

There are ways to type the lines out in a short time ([VSCode Snippets](https://abinjohn.in/code-faster), for example), but it's still a tedious process. And because structure (HTML) and logic (JavaScript) are in two different files, they seem separated, and we constantly have to switch between the files to get the selectors right.

React, on the other hand, combines both HTML and JavaScript using a neat trick called JSX. Take a look at the return value of the `Carousel` Component that generates the HTML in the React version

```javascript
export default function Carousel() {
  const [index, setIndex] = useState(0);
  const [showDetails, setShowDetails] = useState(false);

  let hasPrev = index > 0;
  let hasNext = index < sculptureList.length - 1;

  function handleNextClick() {
    hasNext && setIndex(index + 1);
  }
  function handlePreviousClick() {
    hasPrev && setIndex(index - 1);
  }

  function handleShowDetailsClick() {
    setShowDetails(!showDetails);
  }

  let art = sculptureList[index];
  return (
    <div className="App">
      <button onClick={handlePreviousClick} disabled={!hasPrev}>
        Previous
      </button>
      <button onClick={handleNextClick} disabled={!hasNext}>
        Next
      </button>

      <h2>
        <em>{art.name}</em> by {art.artist}
      </h2>
      <p>
        ({index + 1} of {sculptureList.length})
      </p>
      <button onClick={handleShowDetailsClick}>
        {showDetails ? "Hide details" : "Show details"}
      </button>
      {showDetails && <p>{art.description}</p>}
      <img src={art.url} alt={art.name} style={{ display: "block" }} />
    </div>
  );
}
```

The pros of this approach -

1. The HTML and JavaScript are in one place
    
2. Structure and logic can be wrapped up into independent, reusable pieces of code called 'Components'. Everything related to the carousel - HTML, event listeners, and event handlers- can all be wrapped up in a single `Carousel` Component.
    

### Updating the DOM

Selecting the DOM elements in Vanilla JS is half the story. The other half is updating the DOM; either changing the inner text or the entire HTML by calling the `innerText` and `innerHTML` methods respectively on the selected elements.

This again becomes a tedious process when there are multiple updates to be made. Even for a small application like the one we have, there are close to 6 lines of code to update the DOM. Imagine the case for something a bit more complicated!

Here's the `updateDOM` function in the Vanilla JS version that takes care of the DOM update

```javascript
function updateDOM(scp) {
  scpName.innerText = scp.name;
  currentScp.innerText = `(${index + 1} of ${sculptureList.length})`;
  scpArtistName.innerText = scp.artist;
  scpDetails.innerText = scp.description;
  scpImage.src = scp.url;
  scpImage.alt = scp.alt;
}
```

React has another magic trick up its sleeve to make even this process easy - State. By combining State and JSX, we take advantage of the declarative nature of React and let it handle all the DOM updates.

> In a nutshell, "State" is memory that a React Component has. We can make React 'remember' certain values inside a component. When any of these values get updated, React updates the DOM with the latest values.
> 
> In the example above, I've used the `useState()` hook to play with State.

In the React version, I call the setter function of the `index` State to update it. This makes React call the component with the updated value of `index`. `sculptureList[index]` now points to another item in the array and hence, the returned JSX will be different. React then updates the DOM to reflect the changes.

```javascript
  const [index, setIndex] = useState(0);
  const [showDetails, setShowDetails] = useState(false);

  function handleNextClick() {
    hasNext && setIndex(index + 1);
  }
  function handlePreviousClick() {
    hasPrev && setIndex(index - 1);
  }

  let art = sculptureList[index];
```

## Summary

React definitely helps save a lot of time because of its declarative nature. I'm also falling in love with the concept of bringing HTML and JavaScript to one place using JSX. It helps shift our focus and time to things that demand more attention, like logic, rather than setting up the selectors and event listeners. Do let me know your thoughts! Have you tried something like this?