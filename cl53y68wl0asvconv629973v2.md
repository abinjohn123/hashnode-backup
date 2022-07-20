## Code faster with Snippets (VSCode)

Let's say you're working on a web development project and have to select 10 DOM elements with the infamous `querySelector()` method. Selecting just one element and assigning it to a variable could go like this:
```
const headerEl = document.querySelector('.header')
```
Now imagine repeating that for another 5, 10 or 15 elements! Your fingers will curse you. You could try copying the first line and changing what's necessary, but that isn't an efficient solution, is it?

Enter **custom snippets** - a great way to type out a single line or multiple lines of code easily and with fewer keystrokes. You just type in a trigger word and VSCode replaces the it with a code block we've set. 

## Built-In Snippets
Before we move to custom snippets, it is good to note that VSCode already has built-in snippets for a number of languages, and you may have used them already without realizing it. [(Reference)](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_builtin-snippets)

For example, if you're in a javascript project and started to type out forEach in the editor, VSCode displays a For-Each Loop snippet in the dropdown that appears. Select that and VSCode pastes in a forEach block for you with placeholders that can be jumped through with the Tab key.

Typing and selecting the snippet: ![Typing and selecting the snippet](https://cdn.hashnode.com/res/hashnode/image/upload/v1656763127856/dADBFu3C7.png align="left")
The resulting code block: ![The resulting code block](https://cdn.hashnode.com/res/hashnode/image/upload/v1656763153913/k87QLOIHg.png align="left")

## Custom Snippets
Now it's time to create some snippets for ourselves
* In VSCode, go to File -> Preferences -> Configure User Snippets
* Click on `New Global Snippets File...`
* Give a name for your Snippets file. This file is where you'll be defining your custom snippets.

The snippets file is a JSON formatted file that defines all the custom snippets. Each snippet will have a name and a set of properties that define it. They are:
* *scope* - (optional) Use this to set the languages or projects where the snippets will be identified
* *prefix* - the trigger word that VScode identifies and replaces
* *body* - The code block that will replace the trigger
* *description* - A description of what the snippet does.

### Creating a Snippet
1. Start by creating a new property in the parent object with a blank object as its value. I'm adding one for the querySelector method, and so I'm naming the property 'JS querySelector'
```
{
  "JS querySelector": {}
}
```
2. Define the properties for the snippet in the blank object.
```
  {
    "JS querySelector": {
      "scope": "javascript,typescript",
      "prefix": "dsel",
      "body": ["const ${1:variable} = document.querySelector('${0:selector}')"],
      "description": "Select a DOM element"
    }
}
```
Take a look at the `body` property. Notice `${1:variable}` and `${0:selector}`? Those are placeholders. When VSCode replaces the trigger `dsel` with our codeblock, the placeholder texts will be selected for us to easily change their values. Multiple placeholders can be added with `${2:value}`,` ${3:value}`, etc and the final placeholder will be `${0:value}`. The cursor can be jumped from one placeholder to the next with the tab key which makes changing the values easy and quick.

That's it! Save the file and go back to your javascript/typescript project, type in the magic word `dsel`, and hit the Enter or Tab key.
![screenshot of successful snippet replacement](https://cdn.hashnode.com/res/hashnode/image/upload/v1656768427599/PsnJATq-0.png align="left")
`variable` is selected first. Hit the Tab key to switch to `selector`.
![screenshot of successful snippetreplacement](https://cdn.hashnode.com/res/hashnode/image/upload/v1656768537652/xuS9aWd_J.png align="left")

## The End
Congratulations! You just created your first custom VSCode snippet. Set up snippets for commonly used codeblocks and make your development process quick, easy, and less frustrating. Your fingers will thank you.
Happy coding!