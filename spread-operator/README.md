# Spread Operator

One common concern in Javascript is about object and array references. Often it's useful to make a copy of an object/array rather than destructively modifying it. 

## With Objects

In the past, if we wanted to make a copy of an object, we'd use `Object.assign()`:

```js
const pasta = {
  sauce: 'red',
  garlicky: true
}

const alsoPasta = pasta

console.log(pasta === alsoPasta) // true

const morePasta = Object.assign({}, pasta)

console.log(pasta === morePasta) // false
```

Javascript now has the spread operator, which gives us a more concise syntax for copying objects:

```js
const moreMorePasta = { ...pasta, cheese: 'please' }
console.log(moreMorePasta)
```

## With Arrays

The spread operator can also be used with arrays. It's useful for making copies of arrays as well as concatenating multiple arrays together:

```js
const spices = ['cumin', 'coriander']

const alsoSpices = spices

console.log(alsoSpices === spices) // true

const copySpices = [...spices]

console.log(copySpices === spices) // false

const moreSpices = ['black pepper']

// older syntax
const allTheSpices = spices.concat(moreSpices)

// using spread
const allTheSpicesAgain = [...spices, ...moreSpices]
```

## Resources

- [MDN Spread Syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
- [Object.assign and spread](https://reedbarger.com/how-to-merge-objects-with-the-object-spread-operator/)
