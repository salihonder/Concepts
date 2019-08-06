# Functional Programming with JS
## Intro
1. Keeps functions and data separate 
2. Avoids state change and mutable data 
3. Treats functions as first-class citizens 

Functional programming is about:

- **Isolated functions** - there is no dependence on the state of the program, which includes global variables that are subject to change

- **Pure functions** - the same input always gives the same output

- **Functions with limited side effects** - any changes, or mutations, to the state of the program outside the function are carefully controlled

`Terminology`
**Callbacks** are the functions that are slipped or passed into another function to decide the invocation of that function. You may have seen them passed to other methods, for example in filter, the callback function tells JavaScript the criteria for how to filter an array.

Functions that can be assigned to a variable, passed into another function, or returned from another function just like any other normal value, are called **first class** functions. In JavaScript, all functions are first class functions.

The functions that take a function as an argument, or return a function as a return value are called **higher order** functions.

When the functions are passed in to another function or returned from another function, then those functions which gets passed in or returned can be called a **lambda**.

`Keeps functions and data separate `
```js
// in OOP - Data and functions are together
class Person { 
    constructor (name, age) { 
        this. name = name 
        this. age = age
    }
    
    getOlder() { 
        this. age +=1
    }
}

// at FP Both are seperated
// simply we get an object and without mutating that objet returning a new one
const me = {
    name: "Joe" , 
    age: 30,
}

const house = {age:1, city: "Houston"}

function increaseAge(obj) {
    const objCopy = _.cloneDeep(obj)
    objCopy.age +=1
    return objCopy;
}

const olderMe = increaseAge(me)
const worseHouse = increaseAge(house)
// without oop , superclass, inheritence etc
// we have polymorphism with regards to our data
```

`Avoids state change and mutable data`
```js
// we keep mutating variable and for a big programs it is hard to track
var greeting = "Hello";
greeting = greeting + ", Salih";
greeting = greeting.toUpperCase()
console.log(greeting);

// solution to this use immutable data
const greeting = "Hello";
const longGreeting = greeting + ", Salih";
const longShoutedGreeting = greeting.toUpperCase()
console.log(longShoutedGreeting);
```


`Treats functions as first-class citizens `
we can assign funstions to a variable.
First class functions gets another funstions as an argument and returns a function
```js
const a = function() {...}
a(function() {...})
function a() {
    return function() { ... }
}
```

**Functional vs OOP**
Let's compare a simple shopping list application
in FP, we are not concerned about with defining what everything is and what it can do, but raw data itself and what operations and transformations.
```js
/* OOP */
/************************/
class ShoppingList {
    constructor(items) {
        this.items = items
    }
    addItem(item) {
        this.items.push(item)
    }
}

class ShoppingItem() {
    constructor(name, price) {
        this.name = name
        this.price = price
    }
}

var myShoppingList = new ShoppingList([
    new ShoppingItem("milk",4.99),
    new ShoppingItem("eggs",3.99),
    new ShoppingItem("cola",1.99)
])

// when new item added it mutates the data
myShoppingList.addItem(
    new ShoppingItem("bananas",1.99)
)

if (buyPetMonkey) {
    myShoppingList.addItem(
        new ShoppingItem("bananas", 1.99)
    )
    myShoppingList.sendToPerson()
} else if (guestAreVegans) {
    myShoppingList.remove("milk","eggs")
    myShoppingList.sendToPerson()
} else {
    myShoppingList.sendToPerson()
}

/* FP */
/************************/
function addItem(list, item) {
    reutrn list.concat(item)
}

const myShoppingList = [
    {name: "milk", price: 4.99},
    {name: "eggs", price: 3.99},
    {name: "cola", price: 1.99}
]

// it doesn't mutate the data since return new one
const nextWeekShoppingList = addItem(myShoppingList,
{ name: "bananas", price: 1.99})

const monkeyList =  addItem(myShoppingList,
{ name: "bananas", price: 1.99});
const veganList = removeItems(myShoppingList, "milk", "eggs")

if (buyPetMonkey) {
    sendToPerson(monkeyList)
} else if (guestAreVegans) {
    sendToPerson(veganList)
} else {
    sendToPerson(myShoppingList)
}
/* one side effect of FP is we have to use a new name
for each transformation we perform but it improves code readibility
*/
```

## First-Class Functions
`Assignin functions to variables`
we use function variables instead of normal functions. but what is difference. Just hoisting.
```js
sayHello() // Hello
function sayHello() {
    console.log("Hello")
}
sayHello() // Hello
```

```js
sayHello() // undefined
const sayHello = () => console.log("Hello")
sayHello() // Hello
```

```js
const line = console.log;
line()
line("Hello and welcome to the course!")
line("We seem to be using")
line("a lot of lines here.")
line("Why don't we rename it")
line("to make it look better?")
line("To do that, simply assign the function")
line("'line' to another variable.")
line("You can call it whatever you want.")
line("I called mine 'line' to reflect the")
line("way that I'm using it.")
line()

```

`Passing functions as arguments`
JavaScript let's us do just that by allowing us to pass functions as arguments. In other words instead of passing values we're passing actions. 
```js
// we pass action add function as argument
function someAction(add) {
    return add(5,7)
}
```

```js
// Before - we should optimize all
const x = 1

if (x === 1) {
  sayXis1()
}

if (x === "Bananas") {
  sayXisBananas()
}

if (x < 10 && x > 0) {
  sayXisBetween0And10()
}

function sayXis1() { console.log("x is equal to 1") }

function sayXisBananas() { console.log("x is equal to 'Bananas'") }

function sayXisBetween0And10() { console.log("x is between 0 and 10") }
```
```js
// after
const doIf = (condition, func) => condition ? func() : null;
const x = 1

doIf(x === 1, () => console.log("x is equal to 1"));
doIf(x === "Bananas", () =>  console.log("x is equal to 'Bananas'"))
doIf(x < 10 && x > 0, () =>  console.log("x is between 0 and 10"))
```

```js
// before
function add(x, y) {
  return x + y
}

function subtract(x, y) {
  return x - y
}
```

```js
// after
const callMyAction = (arg1, arg2, func) => {
    return func(arg1, arg2)
}
const add = (x, y) => x + y;
const subtract = (x, y) => x - y;

callMyAction(1,2,add)
callMyAction(1,2,subtract)
```

`Closure and returning functions`

```js

giveMeAFunction()() // Yes

function giveMeAFunction() {
    return function() {
        console.log("I am a function");
    }
}
```

```js
// Closure
function some() {
    var x = 5;
    return function() {
        console.log("x is " + x)
    }
}

var printX = some()
printX() // x is 5
```


```js
// before
// count variable is public
const myCounter = {
  count: 0,
  increment: function() {
    this.count += 1
  },
  currentValue: function() {
    return this.count
  }
}

console.log(myCounter.currentValue())

myCounter.increment()
myCounter.increment()

console.log(myCounter.currentValue())

myCounter.count = 999 // uh oh!

console.log(myCounter.currentValue())
```
```js
// after
// count is private
function createCounter() {
  var count = 0
  return {
    increment: function() {
      count += 1
    },
    currentValue: function() {
      return count
    }
  }
}

var myCounter = createCounter()

console.log(myCounter.currentValue())

myCounter.increment()
myCounter.increment()

console.log(myCounter.currentValue())

console.log(myCounter.currentValue())
```
`Higher-order functions`
A function that takes a function as an argument, returns a function, or both
```js
// before
function printMessageNTimes(n, message) {
  if (n != null && typeof n === 'number') {
    if (message != null && typeof message === 'string') {
      for (var i = 0; i < n; i++) { console.log(message) }
    }
  }
}

function getNthLetter(n, string) {
  if (n != null && typeof n === 'number') {
    if (string != null && typeof string === 'string') {
      return string.charAt(n)
    }
  }
}

function getSubstringOfLength(n, string) {
  if (n != null && typeof n === 'number') {
    if (string != null && typeof string === 'string') {
      return string.substring(0, n)
    }
  }
}

printMessageNTimes(4, "Banana") // prints "Banana Banana Banana Banana"
getNthLetter(2, "Javascript") // 'v'
getSubstringOfLength(5, "Hello and welcome") // "Hello"
```

```js
// after
function doIfSafe(n, message, func) {
  if (n != null && typeof n === 'number') {
    if (message != null && typeof message === 'string') {
      return func(n, message)
    }
  }
}

function printMessageNTimes(n, message) {
  for (var i = 0; i < n; i++) { console.log(message) }
}

function getNthLetter(n, string) {
  return string.charAt(n)
}

function getSubstringOfLength(n, string) {
  return string.substring(0, n)
}

doIfSafe(4, "Banana", printMessageNTimes) // prints "Banana Banana Banana Banana"
doIfSafe(2, "Javascript", getNthLetter) // 'v'
doIfSafe(5, "Hello and welcome", getSubstringOfLength) // "Hello"
```

```js
// improve a bit more
function createSafeVersion(func) {
  return function(n, message) {
    if (n != null && typeof n === 'number') {
      if (message != null && typeof message === 'string') {
        return func(n, message)
      }
    }
  }
}

function printMessageNTimes(n, message) {
  for (var i = 0; i < n; i++) { console.log(message) }
}

function getNthLetter(n, string) {
  return string.charAt(n)
}

function getSubstringOfLength(n, string) {
  return string.substring(0, n)
}

var printMessageNTimesSafe = createSafeVersion(printMessageNTimes)
var getNthLetterSafe = createSafeVersion(getNthLetter)
var getSubstringOfLengthSafe = createSafeVersion(getSubstringOfLength)

printMessageNTimesSafe(4, "Banana") // prints "Banana Banana Banana Banana"
getNthLetterSafe(2, "Javascript") // 'v'
getSubstringOfLengthSafe(5, "Hello and welcome") // "Hello"

getNthLetterSafe("two", "Javascript")

```

## Array Functions
`map`
```js
var numbers = [ 1, 2, 3, 4, 5 ]
var numbersCubed = [ ]

for(var i = 0; i < numbers.length; i++) {
  var element = numbers[i]
  numbersCubed.push(element * element * element)
}
```

```js
var _ = require("lodash")

var numbers = [ 1, 2, 3, 4, 5 ]
var numbersCubed = _.map(numbers, function(element) {
  return element * element * element
})

console.log(numbersCubed) // [1, 4, 9, 16, 25]
```
`filter`
```js
var numbers = [ 1, 2, 3, 4, 5 ]
var evensFromNumbers = [ ]

for (var i = 0; i < numbers.length; i++) {
  var element = numbers[i]
  if (element % 2 === 0) {
    evensFromNumbers.push(element)
  }
}

```

```js
var _ = require("lodash")

var numbers = [ 1, 2, 3, 4, 5 ]
var evensFromNumbers = _.filter(numbers, function(element) {
  return element % 2 === 0
})

console.log(evensFromNumbers)// [2,4]
```

`some and every`
```js
var numbers = [ 2, 4, 6, 8, 10, 12 ]

var arrayContainsEven = false

for (var i = 0; i < numbers.length; i++) {
  var element = numbers[i]
  if (element % 2 === 0) {
    arrayContainsEven = true
  }
}

var arrayIsAllEven = true

for (var i = 0; i < numbers.length; i++) {
  var element = numbers[i]
  if (!(element % 2 === 0)) {
    arrayIsAllEven = false
  }
}
```

```js
var _ = require("lodash")

var numbers = [ 3, 4, 7, 9, 11, 13 ]

var arrayContainsEven = _.some(numbers, function(element) {
  return element % 2 === 0
})

var arrayIsAllEven = _.every(numbers, function(element) {
  return element % 2 === 0
})

console.log("Array contains even?: " + arrayContainsEven)  // true
console.log("Array is all even?: " + arrayIsAllEven) // false
```

`reduce`

```js
var _ = require("lodash")

var shoppingList = [
  { name: "Eggs",    price: 4.99 },
  { name: "Milk",    price: 3.99 },
  { name: "Bananas", price: 2.79 },
  { name: "Beer",    price: 6.99 }
]

var totalCost = _.reduce(shoppingList, function(acc, item) {
  return acc + item.price
}, 0)

console.log(totalCost)

```

## Callbacks
```js
// Synchronous Operations
var file = fs.readFileSync( 'someHugeFile. txt' , 'utf8')
console. log ("That took a long time! ")

// Asynchronous Operations
someAsyncTask(function (result) {
    console.log("The result was: " + result);
})

console.log("I was called right away!")
```

```js
var fs = require('fs')

var fileContents

fs.readFile('message.txt', 'utf8', function(err, data) {
  if (err) throw err
  fileContents = data
  console.log(fileContents)
})

 console.log(fileContents) // undefined
```


## Partials
What partial application is is taking one of those arguments and fixing the value. This gives us a function with one less argument. We can then call this function and it will call our original function with both the argument that we fixed and the new argument that we pass in
```js
function add(x, y, z) {
  return x + y + z
}

function partiallyApply(func, x, y) {
  return function(z) {
    return func(x, y, z)
  }
}

var add5and2 = partiallyApply(add, 5, 2)

console.log(add5and2(3))
console.log(add(5, 2, 3))
```


## Resources
- [learn.freecodecamp.org](https://learn.freecodecamp.org/javascript-algorithms-and-data-structures/functional-programming/)
- [linkedin.com/learning](ttps://www.linkedin.com/learning)