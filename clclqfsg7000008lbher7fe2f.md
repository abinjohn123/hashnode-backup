# JavaScript Modules - How to Import & Export Code

## Introduction

When JavaScript was first introduced in the late 90s, its only purpose was to run basic scripts on the client side and add a sprinkle of interactivity on web pages. It has since undergone massive changes and become synonymous with web development. Along with the adoption and popularity, the volume of JavaScript code on webpages has also increased, leading to lengthier scripts and large codebases.

If you've built projects with lengthy script files, you would know how difficult it is to maintain and debug them, especially if you're visiting the projects after a short break. The concept of modules attempts to solve this problem by splitting code into small, independent chunks that can be imported into the main script file as needed.

Importing and exporting code has existed in NodeJS for a long time, but with ES6, the feature has come to client-side JavaScript, and [most browsers](https://caniuse.com/es6-module) support modules now.

## How do ES6 modules work?

Modules can be thought of as building blocks that work together to run our applications.

When we split code into separate files(modules), we'll have items spread across multiple script files. These items can be variables, objects, arrays, and even functions. To implement ES6 modules, we need *exporting* scripts that export one or more items and *importing* scripts that import these items.

> exporting can be considered as the process of making some item available to the public, while importing is the process of taking an item that is made public.

If you've worked with React on [codesandbox](https://codesandbox.io/), you would have seen two files `App.js` and `index.js`

`App.js` would have a code similar to this:

```javascript
export default function App(){
 //
}
```

and `index.js` would have this line of code at the top:

```javascript
import App from "./App";
```

Here, `App.js` is exporting the `App` function and `index.js` is importing it. But before we dive into the technicalities, let's lay down some fundamentals.

### Implementing module system in Vanilla-JS project

Imagine we're building a banking application. Apart from all the basic banking functionalities, like credit, withdrawals, FDs and RDs, our bank also offers currency conversions. We use a third-party API to handle these conversions.

```javascript
// This is just for reference.
function convertCurrency(amount, base, target){
    return fetch(`imaginary-currency-conversion-api`)
        .then(data => data.json())
        .then(res => console.log(res.value))
}

convertCurrency(100, 'INR', 'USD') // 1.21
```

There are two things to note here -

1. The logic behind the conversion is not important on the grand scale of the application's functionality. We just need to get the converted value.
    
2. The conversion is being handled by a third-party API whose design and implementation are not in our control. The API developers could change the URL, how the queries are sent etc, and that would break the function.
    
3. This function has the potential to be reused in future projects where currency handling is required.
    

Because of these reasons, it is a good idea to move this piece of code into a separate, independent file. The advantages are

* Makes our main banking code easier to read as we get rid of clutter.
    
* Makes debugging easier if the API breaks as the code is now in its own file.
    
* Introduces the possibility of code-reuse.
    

Here's how we do that.

1. The first step is to create a new script file and move the code into it. I'm naming the file 'conversions.js'. This is our module.
    
2. Export the function so that it can be accessed by other scripts. To export any item, add the `export` keyword just before the item is declared/defined.
    
    ```javascript
    // conversions.js
    
    export function convertCurrency(amount, base, target){
        return fetch(`imaginary-currency-conversion-api`)
            .then(data => data.json())
            .then(res => console.log(res.value))
    }
    ```
    
    > NOTE: only top-level values can be exported. Exporting `data` or `res` from `convertCurrency()` will throw an error.
    > 
    > To know more, check out [global scope](https://abinjohn.in/variable-scope#heading-global-scope)
    
3. In the main script file, import the value using the `import` keyword
    
    ```javascript
    // index.js
    
    import { convertCurrency } from './conversions.js'
    ```
    
    The string that follows the `from` keyword is the path of the module file respective to the importing file. Notice how the item names match between the import and export codes. We'll look into them in detail later in the post.
    
4. In the HTML file, set the `type` attribute of the main script tag to `module`
    
    ```xml
    <!-- index.html -->
    
    <script src="index.js" type="module"></script>
    ```
    
5. Use the imported items just like you would in a regular script.
    
    ```javascript
    // index.js
    
    convertCurrency(10, 'USD', 'CAD');
    convertCurrency(100, 'USD', 'INR');
    ```
    

## A closer look into exports

There are primarily two ways to export items in JavaScript - named exports and default exports.

With named exports, the name of the exported module and the imported module must match. We used named export in the code snippet above. A file can have any number of named exports.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676626535295/5b67b56e-2bf6-414a-b616-64b772ef5a4d.png align="center")

With default exports, the name of the imported module and the exported module need not match. This is because there can only be one default export in a file.

If you have multiple named exports, an easy way to export them all in one go is to use a single export statement at the end of the file and mention the items that are to be exported as a comma-separated list enclosed in curly braces.

```javascript
export { item1, item2, item3 }
```

## A closer look into imports

How an item is imported depends on how it is exported. The import statements for Default and named exports work a bit differently and using an incorrect import method will throw errors.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676627684185/1934d67a-b833-4997-9d23-9c13b0a41b8c.png align="center")

When importing named exports, the item names must match the exported item names and should be enclosed in curly braces {}.

With default exports, since there is only one default export per file, wrapping the item in curly braces is not required. We can also give any name for the imported value. That's why `import meow from './cat.js'` will work even though the item exported is `cat()`

Here's an example that mixes both default and named exports

```javascript
// module file

export function cat() { console.log('meow meow') };

export function dog() { console.log('bow bow') };

export default pig() { console.log('oink oink') };
```

```javascript
// importer file

import Piggie, { cat, dog } from './animals.js'
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676627541237/94ae4ba2-7261-48de-ba94-f243fe3617d8.png align="center")

> Having both default and named exports in a single file is possible in JavaScript as long as there is only one default export, but to avoid potential confusion, mixing them is not preferred by many.

## Modifying item names during import/export

It is possible to provide another name for the items during import and export. In the below snippet, `totalPrice` and `totalQuantity` are two items that are being exported, but `totalQuantity` is being exported as `tq`. Any reference to `tq` in the importer code will point to the `totalQuantity` value in the exporter code.

```javascript
// exporter code
export { totalPrice, totalQuantity as tq }

// importer code
import { totalPrice, tq } from './cart.js'
```

Similarly, we can provide another name during the import process. In the snippet below, `totalPrice` is imported as `tp` and any reference to `tp` in the importer code will point to `totalPrice` value in the exporter code.

```javascript
// exporter code
export { totalPrice, totalQuantity }

// importer code
import { totalPrice as tp, totalQuantity } from './cart.js'
```

## Recap

* ES6 modules are a feature in JavaScript that enable developers to split their code into small, independent chunks called modules.
    
* The export and import statements are used to export and import items between javascript files.
    
* There are two types of exports - named and default.
    
* How items are imported depends on how they are exported.
    
* A file can have any number of named exports, but only one default export.
    

---

I hope you got to learn something new today. If you did, do consider sharing this post. Got any questions? Comment away or connect with me on [**Twitter**](https://twitter.com/abin_john98)!