# ES6 For Everyone 
## By Wes Bos
Started 10-08-2017

# Getting Setup
- Get the latest browser which is currently most browsers
- Wes is using Chrome in this course

- Download the starter files
- Feel free to download video files to watch offline if you prefer
- data.js has random-generated data to use in the course

# Module #1 New Variables - Creation, Updating and Scoping
## var Scoping Refresher
- We have 3 ways to declare JS variables:
1. var
- There's no problem if we put var on the 2nd width also
- We can use far to update the variable:
```js
var width = 100;
console.log(width); // 100
width = 200;
console.log(width); // 200
```
  
- var variables are function-scoped
- Width not available outside of the gates (curly brackets)
- The width variable is scoped only to the setWidth() function:
```js
function setWidth() {
    var width = 100;
    console.log(width);
}
setWidth(); // Will work
console.log(width); // Will not work
```
  
- Generally we want to scope locally in a function and return it to a global variable like:
```js
var width;
function setWidth() {
    width = 100;
    console.log(width);
}
setWidth(); // Will work
console.log(width); // Will not work
```

- Variables are generally scoped in functions, but not scoped with if statements
- We just needed dogYears variable temporarily here but that's not the case:
```js
var age = 100;
if(age > 12) {
    var dogYears = age * 7;
    console.log(`you are ${dogYears} dog years old!`);
}
console.log(dogYears); // Leaked outside of the if block to the global window
```
2. let
- Let is scoped to a block, one of its attributes
```js
var age = 100;
if(age > 12) {
    let dogYears = age * 7;
    console.log(`you are ${dogYears} dog years old!`);
}
console.log(dogYears); // Error;
```

3. const
- Const is scoped to a block, one of its attributes
```js
var age = 100;
if(age > 12) {
    const dogYears = age * 7;
    console.log(`you are ${dogYears} dog years old!`);
}
console.log(dogYears); // Error;
```

## let VS const
1. Let can only be declared once inside of a scope (ie a code block)
```js
let points = 50;

let points = 60; // Error;
points = 60; // However, it can be updated
```

2. Let can have the same variable name but can be scoped differently
- The first let is scoped to the window object
- The second let is scoped to the if statement block
- If you change winner variable to var, it will return true
```js
let winner = false;

if (points > 40) {
    let winner = true;
}

winner; // False
```

3. Const variables cannot be changed but the properties can be updated
- I think it can only ever be defined once in the whole scope of the file
```js
const key = "abc123";

key = "abc1234"; // Error;
```
  
- But the properties of the variable inside of the object can be changed
- A good way to think about it is the person will not ever change
- But their attributes can change like their age, hair color, clothes
```js
const person = {
    name: "Tim",
    age: "99"
}

person = {name: "Timothy"} // Error;
person.age = 100; // 100
```
- Another important concept if you want to permanently freeze a variable is to use Object.freeze()
- This is not part of ES6 but an existing method
```js
const person = {
    name: "Tim",
    age: "99"
}
const tim = Object.freeze(person);

tim.age = 100 // It will pass but won't save
tim.age; // Still 99
```

## let and const in the Real World
1. Let and const replaces the IFFE (Immediately-Invoked Function Expression)
```js
var name = "Tim"

name; // "Tim"
```
- IFFEs were scoped that was self-contained so that nothing was leaked into the parent scope
```js
(function() {
    var name = "Tim";
})();

name; // ""

// If you want to get the name, console.log or return it
(function() {
    var name = "Tim";
    console.log(name);
})();
```
- Also, on the window object, there is already a natively defined property called name that refers to current window you're on
- So that's why you get a "" when you call name on the window object
  
- Using let and const solves this leaking to parent scope problem also:
```js
let name = "Tim";
name; // "Tim"
```
```js
{
    let name = "Tim";
}
name; // ""
```
```js
{
    let name = "Tim";
    console.log(name);
}
// "Tim"
```
  
2. Let solves the problem with for loops
- Setting the stage:
```js
for (var = i; i < 10; i ++) {
    console.log(i);
}
// 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
i; // 10;
```
- If we run a setTimout() method:
```js
for (var i = 0; i < 10; i++) {
    console.log(i);
    setTimeout(function() {
        console.log("The number is" + i);
    }, 1000);
}
// You get "The number is 10" ten times
```
- Notice that it runs the for loop to get the results and then runs the console.log() (ie 0, 1, 2, etc.)
- After the console.log() has run, i is now 10, then setTimeout() is ran (ie i = 10)
- So at this point you're just console.log() i as 10 (ie The number is 10, Then number is 10, etc.)
- So if you want to display "The number is i" with i incrementing in value, use the let 
```js
for (let i = 0; i < 10; i++) {
    setTimeout(function() {
        console.log("The number is" + i);
    }, 1000);
}

// The number is 0
// The number is 1
// The number is 2
// And so on...until 9
```
## Temporal Dead Zone
- Variables have to be declared before they are called:
```js
var pizza = "Deep Dish";
console.log(pizza); // "Deep Dish"
```
- But what if you call console.log(pizza) before the variable?
```js
console.log(pizza); // You get undefined
var pizza = "Deep Dish";

// You can access the variable pizza but not the value
// Basically, the creation of the variable is acknowledged
// To access the value, you have to declare the variable at the top
```
  
- With const or let variables, you will get an error:
- Neither have been pre-declared beforehand
```js
console.log(pizza); // Error;
let pizza = "Deep Dish";

console.log(pizza); // Error;
const pizza = "Deep Dish";
```

## Is var Dead? What should I use?
- Two popular opinions exists for the usage of var, let, and const
- You can decide between the two:

### First opinion
1. Use `const` by default
2. Only use `let` if rebinding is needed
3. (`var` shouldn't be used in ES6)

### Second opinion
1. Use `var` for top-level variables that are shared across many (especially larger) scopes
2. Use `let` for localized variables in smaller scopes
3. Refactor `let` to `const` only after some code has been written and you're reasonably sure that you've got a case where there shouldn't be variable reassignment