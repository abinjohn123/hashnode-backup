# JavaScript Modules - How to Import & Export Code

## Introduction

When JavaScript was first introduced in the late 90s, its only purpose was to run basic scripts on the client side and add a sprinkle of interactivity on web pages. It has since undergone massive changes and become synonymous with web development. Along with the adoption and popularity, the volume of JavaScript code has also increased leading to lengthier scripts and large codebases.

If you've built projects with lengthy script files, you would know how difficult it is to maintain and debug them, especially if you're visiting the projects after a short break. The concept of modules attempts to solve this problem by splitting your code into small, independent chunks that can be imported into the main script file as needed. Modules can be thought of as building blocks that work together to run our applications.

Importing and exporting code has existed in NodeJS for a long time, but with ES6, the feature has come to client-side JavaScript, and [most browsers](https://caniuse.com/es6-module) support modules now.

## How do ES6 modules work?

When we split code into separate files(modules), we'll have many items spread across multiple script files. These items can be variables, objects, arrays, and even functions.

To implement ES6 modules, we need *exporting* scripts that export one or more items and *importing* scripts that import these items.

> exporting can be considered as the process of making some item available to the public, while importing is the process of taking an item that is made public.

Now let's work with some code. Below is a snippet from a project of mine used to [adjust timestamps in bulk](https://shift-time.netlify.app/). It enables adding/subtracting a set time from multiple timestamps in one go.

Here's the process of adding 15 seconds to a timestamp 00:02:46 -

1. Convert the timestamp to its seconds equivalent (So 00:02:46 in HH:MM:SS format becomes 166s)
    
2. Add/subtract seconds from it as required (166 + 15 = 181)
    
3. Convert the resultant seconds back to a timestamp (181 becomes 00:03:01)
    

Here's the code snippet for both conversions

```javascript
// index.js

/*
code converts HH:MM:SS time string to its seconds equivalent.
It returns the seconds value if the time string is valid and null if it isn't
*/
const timeToSeconds = (time) => {
  if (!time) return null;

  const [HH, MM, SS] = time
    .split(':')
    .reverse()
    .reduce(
      (acc, cur, i) => {
        acc[2 - i] = Number(cur);
        return acc;
      },
      [0, 0, 0]
    );
  if (HH > 23 || MM > 59 || SS > 59) return null;

  return HH * 3600 + MM * 60 + SS;
};

/*
code converts seconds value to a timestamp string
It returns MM:SS if hours value is 0 and HH:MM:SS if hours > 0
*/
const secondsToTime = (sec) => {
  let hours = Math.floor(sec / 3600);
  let minutes = Math.floor((sec - hours * 3600) / 60);
  let seconds = sec - hours * 3600 - minutes * 60;

  [hours, minutes, seconds] = [hours, minutes, seconds].map(
    (time) => (time < 10 ? '0' + time : '' + time) 
  );

  return hours === '00'
    ? minutes + ':'
    : hours + ':' + minutes + ':' + seconds;
};
```

The logic and nuances behind the conversions are not important in the grand scale of the app's functionality. The app is to add and subtract time from timestamps, so as long as the conversions happen properly, we're good to go. The snippet can also be re-used outside of this application, anywhere where time conversion is required. This makes it a good idea to take this piece of code out into a separate, independent file. Here's how we do that

1. The first step is to create a new script file and move the code into it. I'm naming the file 'conversions.js'
    
2. Export the required items from the file that are to be accessible by other scripts. In 'conversions.js' we have two functions that need to be accessed - `timeToSeconds()` and `secondsToTime()` To export these items, add the `export` keyword just before the item is declared/defined.
    
    ```javascript
    // conversions.js
    
    export function timeToSeconds(timeString){
    //...
    }
    
    export function secondsToTime(sec){
    //...
    }
    ```
    
    > NOTE: only top-level values can be exported. Exporting `hours` or `minutes` from `secondsToTime()` will throw an error
    
3. In the script tag of the main script file, set the `type` attribute to `module`
    
    ```xml
    <!-- index.html -->
    
    <script src="scripts/index.js" type="module"></script>
    ```
    
4. In the main script file, import the values using the `import` keyword
    
    ```javascript
    // index.js
    
    import { timeToSeconds, secondsToTime } from './conversions.js'
    ```
    
    The string that follows the `from` keyword is the path of the module file respective to the importing file. Notice how the item names match between the import and export codes. We'll look into them in detail later in the post.
    
5. Use the imported items just like you would in a regular script.
    
    ```javascript
    // index.js
    
    const secondsValue = timeToSeconds('01:25:22');
    const timevalue = secondsToTime(55)
    ```
    

## A closer look into exports

There are primarily two ways to export items in JavaScript - named exports and default exports.

With named exports, the name of the imported module and the exported module must match. We used named export in the code snippet above. A file can have any number of named exports.

With default exports, the name of the imported module and the exported module need not match. This is because there can only be one default export in a file.

If you have multiple named exports, an easy way to export them all in one go is to use a single export statement at the end of the file and mention the items that are to be exported as a comma-separated list enclosed in curly braces.

```javascript
export { item1, item2, item3 }
```

## A closer look into imports

How an item is imported depends on how it is exported. The import statements for Default and named exports work a bit differently and using an incorrect import method will throw errors.

|  | Export Statement | Import Statement |
| --- | --- | --- |
| Named Export | export function cat() { ... }  
export function dog() { ... } | import { cat, dog } from './animals.js' |
| Default Export | export default function cat() {...} | import meow from './cat.js' |

When importing named exports, the item names must be enclosed in curly braces {} and the names must match the exported item names. With default exports, since there is only one default export per file, wrapping the item in curly braces is not required. We can also give any name for the imported value. That's why `import meow from './cat.js` will work even though the item exported is `cat()`

> Having both default and named exports in a single file is possible in JavaScript as long as there is only one default export, but to avoid potential confusion, mixing them is not preferred by many.

## Modifying item names during import/export

It is possible to provide another name or reference for the items during import and export. In the below snippet, `totalPrice` and `totalQuantity` are two items that are being exported, but `totalQuantity` is being exported as `tq`. Any reference to `tq` in the importer code will point to the `totalQuantity` value in the exporter code.

```javascript
// exporter code
export { totalPrice, totalQuantity as tq }

// importer code
import { totalPrice, tq } from './cart.js'
```

Similarly, we can update the reference during the import process. In the snippet below, `totalPrice` is imported as `price` and any reference to `price` in the importer code will point to `totalPrice` value in the exporter code.

```javascript
// exporter code
export { totalPrice, totalQuantity }

// importer code
import { totalPrice as tp, totalQuantity } from './cart.js' 
```

## Recap

* ES6 modules are a feature in JavaScript that enable developers to split their code into small, independent chunks called modules.
    
* The export and import statements are used to export items from a file and import them into another file, respectively.
    
* There are two types of exports - named and default. How items are imported depends on how they are exported.
    
* A file can have any number of named exports, but only one default export.