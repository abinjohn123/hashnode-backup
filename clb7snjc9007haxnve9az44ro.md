# You Should Use Parallel Promises

Asynchronous JavaScript was introduced to help implement non-blocking code execution. Async-await took it up a notch by making it easier to work with promises and avoid the infamous callback hell.

```javascript
// log every second -> callback hell version

setTimeout(() => {
  console.log('1 second');
  setTimeout(() => {
    console.log('2 seconds');
    setTimeout(() => {
      console.log('3 seconds');
      setTimeout(() => {
        console.log('4 seconds');
        setTimeout(() => {
          console.log('5 seconds');
        }, 1000);
      }, 1000);
    }, 1000);
  }, 1000);
}, 1000);

/* 
OUTPUT:
1 second
2 seconds
3 seconds
4 seconds
5 seconds
*/
```

```javascript
// log every second -> async-await version

const wait = function(time) {
  return new Promise(resolve => setTimeout(resolve, time * 1000))
}

async function clock() {
  try {
    await wait(1);
    console.log('1 second');

    await wait(1);
    console.log('2 seconds');

    await wait(1);
    console.log('3 seconds');

    await wait(1);
    console.log('4 seconds');

    await wait(1);
    console.log('5 seconds');
  } catch (err) {
    console.log(err);
  }
}
clock();

/* 
OUTPUT:
1 second
2 seconds
3 seconds
4 seconds
5 seconds
*/
```

However, one has to be a bit cautious while using `await` because it blocks code i.e when `await` is used, code execution stops until the promise that it is waiting for is settled (either resolved or rejected). This is different from the `.then` and `.catch` methods that were used before it, where code execution would not pause. Regular code execution would continue and `.then` or `.catch` would be called only when the promise had settled.

When you have multiple independent `await` statements in your code block, all those statements become blocking statements, potentially breaking the async nature and the core advantage of using async-await in the first place.

## Real-world example

I'm loading three images from a remote server using the `fetch` method. Since fetch is async and I like modern JavaScript, I take the async-await approach for all asynchronous operations. I then load these images onto the DOM. I also implement a fairly basic timer that works using `Date` to get a rough estimate of how much it takes to finish the entire operation.

Here's the code:

```javascript
const imgContainer = document.querySelector('.img-container');

// simple timer code
let time = '';
function resetTime() {
  time = new Date();
}
function elapsedTime(type) {
  const now = new Date();
  console.log(type, (now - time) / 1000);
}

// fetch photo from server
async function fetchImage(w = 200, h = 300) {
  try {
    const data = await fetch(`https://picsum.photos/${w}/${h}/`);
    return data.url;
  } catch (err) {
    return new Error(err);
  }
}

// load photos to DOM
function loadImages(...sources) {
  sources.forEach((source) => {
    const img = document.createElement('img');
    img.src = source;
    imgContainer.append(img);
  });
}


(async function () {
  resetTime();
  try {
    const photo1 = await fetchImage(700, 800);
    const photo2 = await fetchImage(50, 50);
    const photo3 = await fetchImage(100, 200);
    loadImages(photo1, photo2, photo3);
  } catch (err) {
    console.log(err);
  }
  elapsedTime('one-by-one');
})();
```

I've embedded a sandbox below that runs the code snippet above. Make note of the time it takes to finish the operation. It took between 2 and 3 seconds for me.

<iframe src="https://codesandbox.io/embed/parallel-promises-i23i0z?expanddevtools=1&fontsize=14&hidenavigation=1&theme=dark" style="width:100%;height:500px;border:0;border-radius:4px;overflow:hidden" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

## So what's the problem here?

The three `await` statements block code execution at each step, making the images get fetched only one after the other. However, the three `fetchImage` calls can actually be made independently of one another since fetch was designed to be asynchronous. The only requirement for us is that all three promises should settle before `loadImages` is called so that valid values can be passed in as the arguments.

So, by using multiple `await` statements one after the other, what was supposed to be an async load operation has now become a synchronous, time-consuming one.

This can also be verified by inspecting the Network tab in the Dev Tools when running the code locally. Notice how the activities only happen sequentially.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669898697693/ZpxzKlWPn.png align="left")

If the `loadImage` call is temporarily commented out and the `await` keyword is removed, you can observe the three fetch statements happening asynchronously.

```javascript
(async function () {
  resetTime();
  try {
    const photo1 = fetchImage(700, 800);
    const photo2 = fetchImage(50, 50);
    const photo3 = fetchImage(100, 200);
    // loadImages(photo1, photo2, photo3);
  } catch (err) {
    console.log(err);
  }
  elapsedTime('one-by-one');
})();
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669898631143/_-0QImDwH.png align="left")

## Enter `Promise.all()`

By now, I hope it is established that having multiple independent `await` statements is not a good idea for performance. The solution to this problem is to use the `Promise.all()` method

It takes an iterable of promises as input and returns a single promise. An array of promises is usually passed in as the iterable. The returned promise resolves when all the input promises get fulfilled. It rejects when any one of the input promises reject.

Here's how we modify our existing code

*   Omit the `await` keyword from the three `fetchImage()` calls so that `photo1`, `photo2`, and `photo3` become simple promises that will get a resolved value at a future time.
    
*   Call the `Promise.all()` method and pass in the three promises as an array
    
*   Handle the settled value of this promise.
    

```javascript

    const photo1 = fetchImage(700, 800);
    const photo2 = fetchImage(50, 50);
    const photo3 = fetchImage(100, 200);

    const allPromises = await Promise.all([photo1, photo2, photo3]);
```

If `allPromises` gets fulfilled, the fulfilled value would be an array, with each element being the resolved value of the corresponding input promise. So in our case, the resolved value will be an array of image URLs.

Here's the full code

```javascript
(async function () {
  resetTime();
  try {
    const photo1 = fetchImage(700, 800);
    const photo2 = fetchImage(50, 50);
    const photo3 = fetchImage(100, 200);

    const allPromises = await Promise.all([photo1, photo2, photo3]);
  } catch (err) {
    console.log(err);
  }
  elapsedTime('parallel');
})();
```

Here's the sandbox with the updated code

<iframe src="https://codesandbox.io/embed/parallel-promises-embed-2-uqpcnw?expanddevtools=1&fontsize=14&hidenavigation=1&theme=dark" style="width:100%;height:500px;border:0;border-radius:4px;overflow:hidden" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

Here's how the network tab looks like when I run the code locally

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669902034772/xFCCMR3fG.png align="left")

I tested both versions of the code locally - with and without parallel promises, and checked how long they took. Parallel promises consistently gave significantly better loading times, and were almost 50% faster.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669902583204/oe5hyH-eI.png align="left")

## When Not To Use

Although `Promise.all()` looks like a one-stop solution for performance improvements, it is important to note that this will only be achieved when used on independent promises.

Consider the case where a fetch request is used to receive a JSON response.

```javascript
const fetchResponse = await fetch('randomJSONapi');
const jsonData = await fetchResponse.json();
```

Even though two `await` statements are used one after the other, here it is required that the first `await` gets settled before the second one is called.

## Wrapping Things Up

Returning promises in parallel is definitely a great way to improve code performance. A similar method `Promise.allSettled()` has been introduced as part of ES2022, with the difference being that `allSettled()` will not reject if one of the input promises reject. Instead, it will return the fulfilled values of the resolved input promises.

I hope you got to learn something new today. Do let me know what you think as a comment or connect with me on [Twitter](https://twitter.com/abin_john98). I'd love to chat!

## Resources -

1.  [Learn more about Promises in JavaScript](https://abinjohn.in/javascript-promise)
    
2.  [MDN documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)
    
3.  [Another great blog post](https://dmitripavlutin.com/promise-all/)