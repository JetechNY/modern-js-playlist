# Destructuring

From the MDN article on [Destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment):

> The destructuring assignment syntax is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.

## Object Destructuring

Let's say we have an object and want to create a variables for some of its properties. We could to it the long way:

```js
const student = { name: "Duane", grade: 96 }

const name = student.name
const grade = student.grade
console.log(name) // "Duane"
console.log(grade) // 96
```

But object destructuring gives us a shorter syntax to accomplish the same thing.

```js
const student = { name: "Duane", grade: 96 }

const { name, grade } = student
console.log(name) // "Duane"
console.log(grade) // 96
```

In the example above, `const { name, grade } = student` will do the following:

- Create two new variables, `name` and `grade`
- Assign the value of `student.name` to the `name` variable, and `student.grade` to the `grade` variable

One way to think about this syntax: on the left-hand side of the equals sign, you're setting up a *pattern* to match from the right-hand side. Any keys from the object that fit into this pattern will be assigned to variables.

Here's a practical example of where this code might help. We have a function `renderPizza` that accepts a `pizza` object, and we're using that object to create some HTML markup.

```js
const pizzaContainer = document.querySelector("#pizza")

function renderPizza(pizza) {
  pizzaContainer.innerHTML = `
    <h3>Your Pizza Awaits!</h3>
    <p>Cheese: ${pizza.cheese ? "Yes" : "No"}</p>
    <p>Size: ${pizza.size}</p>
    <p>Toppings:</p>
    <ul>
      ${pizza.toppings.map(topping => `<li>${topping}</li>`).join("")}
    </ul>
  `
}

const mushroomPizza = {
  toppings: ["mushrooms", "pepperoni"],
  cheese: true,
  size: "large"
}

renderPizza(mushroomPizza)
```

We can shorten this with destructuring to clean up the string:

```js
function renderPizza(pizza) {
  const { cheese, size, toppings } = pizza
  pizzaContainer.innerHTML = `
    <h3>Your Pizza Awaits!</h3>
    <p>Cheese: ${cheese ? "Yes" : "No"}</p>
    <p>Size: ${size}</p>
    <p>Toppings:</p>
    <ul>
      ${toppings.map(topping => `<li>${topping}</li>`).join("")}
    </ul>
  `
}
```

We can even do destructuring in the parameters of the function:

```js
function renderPizza({ cheese, size, toppings }) {
  pizzaContainer.innerHTML = `
    <h3>Your Pizza Awaits!</h3>
    <p>Cheese: ${cheese ? "Yes" : "No"}</p>
    <p>Size: ${size}</p>
    <p>Toppings:</p>
    <ul>
      ${toppings.map(topping => `<li>${topping}</li>`).join("")}
    </ul>
  `
}

const mushroomPizza = {
  toppings: ["mushrooms", "pepperoni"],
  size: "large"
}
renderPizza(mushroomPizza)
```

This has an added advantage of making it very clear what kind of objects this function expects (and what keys are needed on that object).

You can also assign default values to destructured variables, so that if the object you're destructuring doesn't have that property, it will be assigned to the default value:

```js
function renderPizza({ cheese = true, size, toppings }) {
  pizzaContainer.innerHTML = `
    <h3>Your Pizza Awaits!</h3>
    <p>Cheese: ${cheese ? "Yes" : "No"}</p>
    <p>Size: ${size}</p>
    <p>Toppings:</p>
    <ul>
      ${toppings.map(topping => `<li>${topping}</li>`).join("")}
    </ul>
  `
}
```

One other example of where it's helpful to use destructuring is with functions that accept objects with a lot of properties, such as event listeners:

```js
let likes = 0
const buttonContainer = document.querySelector("#buttons")

buttonContainer.addEventListener("click", function({ target }) {
  if (target.matches(".upvote")) {
    likes++
  } else if (target.matches(".downvote")) {
    likes--
  }
  
  console.log(likes)
})
```

Here, we're destructuring the `target` property from the event object, since that's the only property we care about in this function.

We can also rename the variable when we destructure it. For example: 

```js
buttonContainer.addEventListener("click", function({ target: element }) {
  if (element.matches(".upvote")) {
    likes++
  } else if (element.matches(".downvote")) {
    likes--
  }
  
  console.log(likes)
})
```

Here, we're assigning the `target` property of the event object to a parameter name of `element`, so in the body of the function, `element` will refer to the `target` property.

You can also work with nested objects:

```js
const user = {
  name: "Duane",
  phones: {
    cell: "555-123-4567",
    office: "555-123-4568"
  }
}

const { name, phones: { cell, office } } = duane
console.log(name)
console.log(office)
```

## Array Destructuring

Arrays can also be destructured, using a slightly different syntax. Take this example:

```js
const user = "Duane Watson"
const namesArray = user.split(" ") // ["Duane", "Watson"]
const firstName = namesArray[0]     
const lastName = namesArray[1]     

console.log(firstName) // "Duane"
console.log(lastName) // "Watson"
```

> Aside: it's generally a Bad Idea to assume that everyone's name will follow this first name/last name pattern; this is just an example!

We can use the array destructuring syntax to get the first name and last name more succinctly:

```js
const user = "Duane Watson"
const namesArray = user.split(" ") // ["Duane", "Watson"]

const [firstName, lastName] = namesArray // Array destructuring

console.log(firstName) // "Duane"
console.log(lastName) // "Watson"
```

In the example above `const [firstName, lastName] = namesArray` will take the first element from the namesArray and assign it to the `firstName` variable, and assign the second element to the `lastName` variable.

Just like with object destructuring, you can assign default values:

```js
const user = "Jane"
const namesArray = user.split(" ") // ["Jane",]

const [firstName, lastName = "Doe"] = namesArray // Array destructuring

console.log(firstName) // "Jane"
console.log(lastName) // "Doe"
```

You can also skip over elements from the array if you only need elements from later on in the array:

```js
const user = "Duane Watson"
const namesArray = user.split(" ")

const [, lastName] = namesArray

console.log(lastName) // "Watson"
```

A nice use for array destructuring is for functions that always return an array with the same number of elements. You can almost think of these functions as having multiple return values!

```js
function getRgb(colorString) {
  const rgbString = colorString.replace("rgb(", "").replace(")", "")
  return rgbString.split(",")
}

const [r,g,b] = getRgb("rgb(200,123,20)")
console.log(r) // "200"
console.log(g) // "123"
console.log(b) // "20"
```

## Conclusion

Try using object destructuring any time you find yourself accessing lots of properties from the same object; it'll save you some keystrokes and can help make your code more readable. 

You'll definitely see examples of destructuring in React and lots of modern Javascript code, so it's great to get some practice using it now!

## Resources

- [javascript.info](https://javascript.info/destructuring-assignment)
- [Object and Array Destructuring](https://ui.dev/object-array-destructuring/)