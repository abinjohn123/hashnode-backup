# Two Smooth-Scrolling Techniques

Here is a starter file to make following this guide easier. Click on the 'Edit on Codepen' text on the top-right of the embed below to open the file in a separate tab.

<iframe height="300" style="width: 100%;" scrolling="no" title="Untitled" src="https://codepen.io/abinjohn/embed/oNyWmQx?default-tab=html%2Cresult&editable=true" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/abinjohn/pen/oNyWmQx">
  Untitled</a> by Abin John (<a href="https://codepen.io/abinjohn">@abinjohn</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

## Using Pure CSS

To implement smooth scrolling via pure CSS, we make use of anchor tags and ID attributes. 

When an anchor tag is clicked, the browser directs us to the URL specified in its `href` attribute. The URL can point to both external sites as well as specific sections of the web page(internal linking). 

We first give IDs to elements that we want to scroll into and mention those IDs in the `href` attributes of the anchor tags. We prefix the IDs with the # symbol that lets the browser know that the link is from the current page.
```html
<nav>
  <ul class="links">
    <li><a href="#sec-1">section 1</a></li>
    <li><a href="#sec-2">section 2</a></li>
    <li><a href="#sec-3">section 3</a></li>
    <li><a href="#sec-4">section 4</a></li>
    <li><a href="#sec-5">section 5</a></li> 
  </ul>
</nav>
```
Doing so enables click-to-scroll, i.e. clicking on any of the anchor tags will now scroll the page to the element containing the ID specified in their `href` attributes. But this scroll is a jump-scroll, i.e. the page directly jumps to the appropriate section. To implement a smooth scroll, we modify the scroll behavior in the CSS.

We set the `scroll-behavior` property to `smooth` for the element or container that needs the smooth-scrolling behavior. In most cases, the entire web page scrolls, so the property will be set for the `html` element. But in the starter code that we're working on, scrolling happens inside the `main` tag which has a fixed height and width. So the property is set for that element.

```css
main {
  height: 100%;
  margin: 20px;
  overflow-y: scroll;
  scroll-behavior: smooth;
}
```

Clicking on the anchor tags now will scroll the view smoothly into the appropriate sections

## Using JavaScript
We can get rid of the anchor tags when taking the JavaScript route, as it is then possible to trigger a scroll from any element.

We still need an identifier to know which element to scroll into, and we can make use of custom data attributes for that. 

Set a custom attribute called `target` for each of the nav items with values ranging from 1 to 5. You'll understand why in a moment.

```js
<nav>
  <ul class="links">
    <li data-target="1">section 1</li>
    <li data-target="2">section 2</li>
    <li data-target="3">section 3</li>
    <li data-target="4">section 4</li>
    <li data-target="5">section 5</li> 
  </ul>
</nav>
```
Here's how we'll implement smooth scrolling with JS
1. Add a click event listener to the list items. I'm adding one listener on the `ul` tag and listening to the click events on all the list items via [event delegation](https://abinjohn.in/event-delegation-in-js)
2. In the click handler, retrieve the target information from the clicked element
3. Select the element corresponding to the retrieved target information
4. use `element.scrollIntoView({behavior: 'smooth'})` to smoothly scroll into the element

```js
const navLinks = document.querySelector('.links');

function navClickHandler(event) {
  const clicked = event.target.closest('li');
  const targetId = clicked.dataset.target;
  
  const targetEl = document.querySelector(`#sec-${targetId}`);
  targetEl.scrollIntoView({behavior: 'smooth'})
  }

navLinks.addEventListener('click', navClickHandler)
```