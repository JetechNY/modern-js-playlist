#  Import/Export and ES Modules

If you have code broken up across multiple files, you can use the ES Module system to manage access to your code across different files! 

> NOTE: using ES modules requires you to be running your frontend on a server! You can't just open the html file from the file system. Use a tool like [serve](https://www.npmjs.com/package/http-server) or the VSCode [Live Server extension](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) to run your frontend code if you'd like to use modules.

ES Modules help solve a couple of problems. Separating your code across multiple files makes it easier to maintain and less cluttered, but if you're relying on loading in multiple script files in your HTML file, knowing what order they need to go in can be tricky. ES Modules let us split our code across multiple files and ensures that we have access to all the code we need, when we need it.

Modules also help enforce *separation of concerns* in our project. Variables declared in modules are only accessible from within the module unless we specifically export them, so we don't needlessly clutter up the global scope.

Let's say we have a project set up like this: 

```
├── utils/
│   ├── calculate.js
│   └── math.js
├── index.js
└── index.html
```

Our Javascript code is split across three files: `utils/calculate.js`, `utils/math.js`, and `index.js`. 

If the files are imported in the wrong order, we have a problem: code in one file might *depend* on code in another file! Let's see how we can fix that.

To get started using ES6 Modules, we'll just add a script tag for our main `index.js` file in `index.html` and give it a type of module:

```html
<script src="index.js" type="module">
```

> Note: module scripts are deferred automatically, so no need to use defer!

The `index.js` file is now the 'entry point' into the rest of our application. So any code from other files must be imported into this file so we can use it.

In order to `import` code, first we'll need to specify what variables we want to `export` from each of our utility script files:

```js
// in ./utils/math.js

const add = (num1, num2) => num1 + num2

const subtract = (num1, num2) => num1 - num2

export { add, subtract }
```

And in order to use these variables in another file, we'll need to `import` them using the exported variable names and the relative path to the other Javascript file:

```js
// in ./utils/calculate.js
import { add, subtract } from './math.js'

function calculate(num1, num2, operator) {
  switch (operator) {
    case "+":
      return add(num1, num2) // using imported add fn
    case "-":
      return subtract(num1, num2) // using imported subtract fn
    default:
      return ""
  }
}
```

In the example above, we're using *named exports* to specify what variables we're exporting. You can also specify a default export. For example:

```js
// in ./utils/calculate.js
import { add, subtract } from './math.js'

function evaluate(num1, num2, operator) {
  switch (operator) {
    case "+":
      return add(num1, num2) // using imported add fn
    case "-":
      return subtract(num1, num2) // using imported subtract fn
    default:
      return ""
  }
}

export default evaluate
```

To import a default export:

```js
// in ./index.js
import evaluate from './utils/calculate.js'

console.log(evaluate(2, 2, "+"))
```

## Resources

- [MDN on ES Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
- [ES Modules explained in Comics](https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/)
