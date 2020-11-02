# async/await

ES2017 introduced **async functions** and the **await** keyword for writing asynchronous code with Promises, as an alternative syntax for using `.then`. It's basically syntactic sugar on top of Promises, but it can make code that is easier to read and reason about.

Let's take this code for making a fetch request as an example:

```js
function getFoxPic() {
  fetch("https://randomfox.ca/floof/")
    .then(response => response.json())
    .then(data => console.log(data))
}
```

We can make this function an `async` function by adding the `async` keyword:

```js
async function getFoxPic() {
  fetch("https://randomfox.ca/floof/")
    .then(response => response.json())
    .then(data => console.log(data))
}
```

You can also write it as an arrow function:

```js
const getFoxPic = async () => {
  fetch("https://randomfox.ca/floof/")
    .then(response => response.json())
    .then(data => console.log(data))
}
```

Anywhere we'd normally use `.then` and a callback function to handle a Promise resolving, we can use `await` instead:

```js
const getFoxPic = async () => {
  const response = await fetch("https://randomfox.ca/floof/")
  const data = await response.json()
  console.log(data)
}
```

This syntax can make our code a lot easier to understand, since there's no more `.then` and no more callbacks to worry about.

One **very important** thing to understand about `async` functions: they will **ALWAYS** return a Promise:

```js
const getFoxPic = async () => {
  const response = await fetch("https://randomfox.ca/floof/")
  const data = await response.json()
  return data
}

console.log(getFoxPic())

getFoxPic()
  .then(data => console.log(data))
```

If you want to add some error handling to your async functions, you can use a `try/catch` block:

```js
const getFoxPic = async () => {
  try {
    const response = await fetch("https://randomfox.ca/floof/")
    if (response.ok) {
      const data = await response.json()
      return data
    } else {
      throw new Error(`Error in fetch: ${response.statusCode}`)
    }
  } catch(e) {
    console.error(e)
  }
}
```

A couple important notes:

- `await` can only be used in `async` functions
  - so `await` can't be used in the global scope
- `async/await` makes your code *look* more synchronous, but you're still dealing with asynchronous code - so be careful!

## Resources

- [javascript.info on async/await](https://javascript.info/async-await)
- [MDN on async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)