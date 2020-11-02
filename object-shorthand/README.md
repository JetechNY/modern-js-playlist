# Object Property Tricks

We'll show a couple of tricks for working with properties on objects.

## Object Property Shorthand

Here's how we can create a new object in Javascript:

```js
const pizza = 'pepperoni'
const restaurant = 'sbarros authentic New York Pizza™️'

const pizzaObj = {
  pizza: pizza,
  restaurant: restaurant
}

console.log(pizzaObj)
```

If the properties on the object match the names of variables, we can use this shorthand syntax:

```js
const pizza = 'pepperoni'
const restaurant = 'sbarros authentic New York Pizza™️'

const pizzaObj = {
  pizza,
  restaurant
}

console.log(pizzaObj)
```

Both versions will create a new object with `pizza` and `restaurant` as the keys; the second approach gives us a nice, concise syntax.

You might use this in functions that return an object:

```js
function createUser(username, password) {
  return {
    username,
    password
  }funct
}

const user = createUser("Duane", "123")
console.log(user) // { username: "Duane", password: "123" }
```

## Computed Property Names

Javascript has always given you a few different ways to access keys on objects, the bracket `[]` notation or the dot `.` notation:

```js
const student = {
  name: "Liza",
  grade: 88
}

console.log(student.name) // "Liza"
console.log(student["grade"]) // 88
```

The bracket notation will also let you access properties dynamically:

```js
const student = {
  name: "Liza",
  grade: 88
}

let key = "name"
console.log(student[key]) // "Liza"
```

In modern Javascript (as of ES6), you can also dynamically *create* objects with computed properties:

```js
let key = "username"

const user = {
  [key]: "Duane"
}

console.log(user) // { username: "Duane" }
```

Any value inside the brackets will be coerced into a string:

```js
function createObject(key, value) {
  return { 
    [key]: value
  }
}

createObject("name", "Liza") // { name: "Liza" }
createObject({}, "wat")      // { [object Object]: "wat" }
```

## Resources
- [Object Property Shorthand](https://alligator.io/js/object-property-shorthand-es6/)
- [Computed Property Names](https://ui.dev/computed-property-names/)