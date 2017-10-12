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
for (var i = 0; i < 10; i ++) {
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
// The number is 10
// The number is 10
// The number is 10
// And so on..repeated until ten times
```
- Notice that it runs the for loop to get the results and then runs the console.log() (ie 0, 1, 2, etc.)
- After the console.log() has run, i is now 10, then setTimeout() is ran
- So at this point you're just console.log() i as 10 
- So if you want to display "The number is i" with i incrementing in value, use let 
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

# Module #2 Function Improvements: Arrows and Default Arguments
## Arrow Functions Introduction
- Arrow functions were introduced in ES6
- Some features they provided:
1. More concise than regular functions
2. They have implicit returns
3. They don't rebind "this" when used inside of another function
  
- So to set the stage:
```js
const names = ["wes", "kait", "lux"];

const fullNames = names.map(function(name) {
    return `${name} bos`;
});

console.log(fullNames); // ["wes bos", "kait bos", "lux bos"]
```
  
- Now to change it to arrow functions:
```js
// Original
const fullNames = names.map(function(name) {
    return `${name} bos`;
});

// 1st pass: Remove function and add fat arrow
const fullNames2 = names.map((name) => {
    return `${name} bos`;
})

// 2nd pass: Remove the parenthesis on the parameter
// You can choose to use them with one argument which is okay too
// If you have multiple arguments, you should use the parenthesis
const fullNames3 = names.map(name => {
    return `${name} bos`;
})

// 3rd pass: Create an implicit return
// Remove the return and brackets
const fullNames4 = names.map(name => `${name} bos`);

// 4th pass: Remove argument if there are none
// Still need empty parenthesis
const fullNames5 = names.map(() => `cool bos`);
```
### Arrow functions are anonymous functions
- Unlike a named function, you can't give the function a name:
```js
function sayMyName(name) {
    alert(`Hello ${name}`);
}
```
- However, you can put it in a variable:
```js
const sayMyName = (name) => { alert(`Hello ${name}!`) }

sayMyName("Wes"); // "Hello Wes!"
```

## More Arrow Function Examples
### Racing example
- Setting the stage:
```js
const race = "100m Dash";
const winners = ["Hunter Gath", "Singa Song", "Imda Bos"];

// Ideally, information is easier to handle in an object
{
    name: "Wes Bos",
    place: 1,
    race: race
}
```
  
- So now we want to loop over the winners array and return an object
- Notice that we used 2 parameters, must have parenthesis
- If we want to return an object implicitly, must have parenthesis around that object
- Otherwise, it is treated as brackets of a function
- We also have to add +1 on to i because it's based off 1st, 2nd, 3rd place and not 0
- Also, use console.table(), it's great
```js
const race = "100m Dash";
const winners = ["Hunter Gath", "Singa Song", "Imda Bos"];

const win = winners.map( (winner, i) => ({name: winner, race: race, place: i + 1}) );

win; // Returns 3 objects
console.table(win); // Better way to return objects in a table
```
- Another new feature in ES6:
```js
({name: winner, race, place: i + 1}) // You can also refactor "race: race" to "race"
```

### Age Example
- We want to filter people older than 60 years old
- Normally we use an "if" statement, but we can use .filter() instead
- It looks strange with => and >= in the same line:
```js
const ages = [23,62,45,234,2,62,234,62,34];

const old = ages.filter(age => age >= 60);
console.log(old); // [62,234,62,234,62]
```

## Arrow Functions and `this`
- When using arrow functions, the "this" keyword does not get rebound
- In this example, Wes has a transition on a box that grows in size with text sliding in
- Setting the stage:
```js
const box = document.querySelector('.box');
box.addEventListener('click', function() {
    console.log(this);
    })

// "this" refers to box which refers to .box, so it works
```
- If we change to an arrow function:
```js
const box = document.querySelector('.box');
box.addEventListener('click', () => {
    console.log(this);
    })

// "this" will refer to the window object
// Because using arrow functions refers to the parent scope, this case, the window object
// The arrow function is not inside of the another block so it takes on the parent object
```
- So now changing it back to function(), we want to toggle the box to open up:
```js
const box = document.querySelector('.box');
box.addEventListener('click', function() => {
    this.classList.toggle('opening');
    })
```
- Also add the text to slide in after half a second:
```js
const box = document.querySelector('.box');
box.addEventListener('click', function() => {
    this.classList.toggle('opening');
    setTimeout(function() {
        console.log(this);
        this.classList.toggle('open');
        }, 500);
    });

// However, we get an error inside the setTimeout()
// "this" refers to the window object again?
// Since we started a new function, it is not bound to anything
// "this" refers to the parent scope defined outside for that reason
```
- We change the setTimeOut function to an arrow function to fix this:
```js
const box = document.querySelector('.box');
box.addEventListener('click', function() => { // "this" from following line refers to box
    this.classList.toggle('opening'); // setTimeout's "this" refers here
    setTimeout(() => {
        console.log(this); // "this" refers to the this next to .classList
        this.classList.toggle('open');
        }, 500);
    });

// Arrow functions do not change the value of "this"
// It inherits the value of "this" from the parent function
// Reason is because it is inside of another function
// We do not have to worry about the scope changing
// It will now take on the value of "this" in this.classList, which in turn refers to the box 
```

### Solving the reverse animation
- You can add change your code:
- We will look further in the destructuring part of the course
- You can use ES6 syntax to switch the variables
```js
const box = document.querySelector('.box');
box.addEventListener('click', function() => {
    let first = 'opening';
    let second = 'open';

    if(this.classList.contains(first)) {
        [first, second] = [second, first] // New ES6 syntax
    }
    this.classList.toggle(first);
    setTimeout(function() => {
        console.log(this);
        this.classList.toggle(second);
        }, 500);
    });

// The logic here is that if the box already has the class opening,
// It switches the variables and does the reverse process
```

## Default Function Arguments
- Default Function Arguments make your code much more readable and maintainable
- Setting the state for a calculate bill example:
```js
function calculateBill(total, tax, tip) {
    return total + (total * tax) + (total * tip);
}

const totalBill = calculateBill(100, 0.13, 0.15);
console.log(totalBill); // 128

const totalBill = calculateBill(100);
console.log(totalBill); // NaN
// So what do we do if we don't know the tax and tip rate and just assume?
// Those values for tax and tip will be undefined if you leave them blank
```
- Normally, what we would do is 1 of 2 ways:
- The problem is the code is cumbersome
```js
// Method 1
function calculateBill(total, tax, tip) {
    if (tax === undefined) {
        tax = 0.13;
    }
    if (tip === undefined) {
        tip = 0.15;
    }
    return total + (total * tax) + (total * tip);
}

// Method 2
function calculateBill(total, tax, tip) {
    tax = tax || 0.13;
    tip = tip || 0.15;

    return total + (total * tax) + (total * tip);
}
```

### Setting default function arguments
- Instead now in ES6, we can just set the values in the parameters:
- So if nothing is passed in for those parameters, those default values will pass in
```js
function calculateBill (total, tax = 0.13, tip = 0.15) {
    return total + (total * tax) + (total * tip);
}
```
- Can we leave one of the arguments emtpy?
- No, you have to set it as undefined
```js
function calculateBill (total, tax = 0.13, tip = 0.15) {
    return total + (total * tax) + (total * tip);
}

const totalBill = calculateBill(100, , 0.25);
console.log(totalBill); // Error;
```
- Instead, just explicitly pass undefined:
```js
function calculateBill (total, tax = 0.13, tip = 0.15) {
    return total + (total * tax) + (total * tip);
}

const totalBill = calculateBill(100, undefined, 0.25);
console.log(totalBill); // 138;
```

## When NOT to use an Arrow Function
- These are 4 situations when you don't want to use the arrow function
### 1. When you really need `this`:
- This function turns a button yellow on and off
```js
// The wrong way
const button = document.querySelector('#pushy');
button.addEventListener('click', () => {
    this.classList.toggle('on');
});

// "this" was bound to the window object because it was not inside of a function
// If you console.log(this), you will find that it's the window

// The right way
const button = document.querySelector('#pushy');
button.addEventListener('click', function() {
    this.classList.toggle('on');
});
```
  
### 2. When you need a method to bind to an object:
```js
// The wrong way
const person = {
    points: 23,
    score: () => {
        this.points++;
    }
}

console.log(person.points); // 23
console.log(person.score()); // Should be 24
console.log(person.score()); // Should be 25
console.log(person.score()); // Should be 26
console.log(person.points); // Still 23

// "this" keyword was not bound to the object so you need to use function()
// If you console.log(this), you will find that it's the window

// The right way
const person = {
    points: 23,
    score: function() {
        this.points++;
    }
}

console.log(person.points); // 23
console.log(person.score()); // Should be 24
console.log(person.score()); // Should be 25
console.log(person.score()); // Should be 26
console.log(person.points); // 26

// There's a refactor for a method on an object but we will visit this more later
const person = {
    points: 23,
    score() {
        this.points++;
    }
}
```

### 3. When you need to add a prototype method
- We have not learned about class yet
- We have a prototype to summarize the type of car and color
```js
// The wrong way
class Car {
    constructor (make, colour) {
        this.make = make;
        this.colour = colour;
    }
}

const beemer = new Car("bmw", "blue");
const subie = new Car("Subaru", "white");

Car.prototype.summarize = () => {
    return `this care is a ${this.make} in the colour ${this.colour}`;
}

subie; // Car {make: "Subaru", colour: "white"}
beemer; // Car {make: "bmw", colour, "blue"}
subie.summarize(); // "This car is undefined in the colour undefined"

// This was not bound to the Car constructor so we need to use function() here

// The right way
class Car {
    constructor (make, colour) {
        this.make = make;
        this.colour = colour;
    }
}

const beemer = new Car("bmw", "blue");
const subie = new Car("Subaru", "white");

Car.prototype.summarize = function() {
    return `this care is a ${this.make} in the colour ${this.colour}`;
}

subie.summarize(); // "This car is Subaru in the colour white"
bmw.summarize(); // "This car is bmw in the colour blue"
```

### 4. When you need arguments object
- We are trying to return strings in an array with order of child number
- We have an "arguments" keyword that returns keywords from function
```js
// The wrong way
const orderChildren = () => {
    const children = Array.from(arguments);
    return children.map((child, i) => {
        return `${child} was child #${i + 1}`;
    })
    console.log(arguments); // Error;
}

// You do not get arguments if you use an arrow function

// The right way
const orderChildren = function() => {
    const children = Array.from(arguments);
    return children.map((child, i) => {
        return `${child} was child #${i + 1}`;
    })

    console.log(arguments); // Works
}
```

## Arrow Functions Exercises
- There are 2 exercises

### Exercise 1
- Note that we are just returning seconds at the end
- Don't worry about converting it back to time in hours and minutes
- Code:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Arrow Functions</title>
</head>
<body>

<ul>
  <li data-time="5:17">Flexbox Video</li>
  <li data-time="8:22">Flexbox Video</li>
  <li data-time="3:34">Redux Video</li>
  <li data-time="5:23">Flexbox Video</li>
  <li data-time="7:12">Flexbox Video</li>
  <li data-time="7:24">Redux Video</li>
  <li data-time="6:46">Flexbox Video</li>
  <li data-time="4:45">Flexbox Video</li>
  <li data-time="4:40">Flexbox Video</li>
  <li data-time="7:58">Redux Video</li>
  <li data-time="11:51">Flexbox Video</li>
  <li data-time="9:13">Flexbox Video</li>
  <li data-time="5:50">Flexbox Video</li>
  <li data-time="5:52">Redux Video</li>
  <li data-time="5:49">Flexbox Video</li>
  <li data-time="8:57">Flexbox Video</li>
  <li data-time="11:29">Flexbox Video</li>
  <li data-time="3:07">Flexbox Video</li>
  <li data-time="5:59">Redux Video</li>
  <li data-time="3:31">Flexbox Video</li>
</ul>

<script>

  // Select all the list items on the page and convert to array
  const items = Array.from(document.querySelectorAll('[data-time]'))

  // Filter for only the elements that contain the word 'flexbox'
  const filtered = items
    .filter(item => item.innerHTML.includes("Flexbox"))

    // map down to a list of time strings
    .map(item => item.dataset.time)

    // map to an array of seconds
    .map(item => {
      const parts = item.split(":").map(part => parseFloat(part));
      return (parts[0] * 60) + parts[1];
    })  
    // reduce to get total
    .reduce( (totalTime, seconds) => (totalTime + seconds), 0);

    // 🔥 This can also be done in a single .reduce(), but we're practicing arrow functions here, so chain them!
</script>
</body>
</html>
```

### Exercise 2
- Code:
```js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Arrow Functions</title>
</head>
<body>

<script>
    // 1. Given this array: `[3,62,234,7,23,74,23,76,92]`, use the array filter method and an arrow function to create an array of the numbers greater than `70`
    const numbers = [3, 62, 234, 7, 23, 74, 23, 76, 92];
    numbers.filter(age => age > 70);
</script>
</body>
</html>
```

# Module #3 Template Strings
## Template Strings Introduction
- In other languages we can stick variables right into the string
- Now in JS, it's possible with template strings (aka template literals)s
- Here we convert a string with variables under one tag
- Backticks are new in ES6 and initiates template strings
```js
const name = "Snickers";
const age = 2;
const sentence = `My dog ${name} is ${age * 7} years old.`;

console.log(sentence); // "My dog Snickers is 14 years old."
```

## Creating HTML fragments with Template Literals
### 1. Writing multiple lines of codes easily
- We can now type all our text without using multiple lines
- In the past:
```js
const person = {
    name: "Wes",
    job: "Web Developer",
    city: "Hamilton",
    bio: "Wes is a really cool guy that loves to teach web development!",
};

var text = "hello there, \
    how are you \
";
```
- Now we can use multi-line strings:
- It will take care of the return's and spacing
```js
const markup = `
    <div class="person">
        <h2>
            ${person.name}
            <span class="job">${person.job}</span>
        </h2>
        <p class="location">${person.city}</p>
        <p class="bio">${person.bio}</p>
    </div>
`

document.body.innerHTML = markup;
```
### 2. Nesting strings inside of each other
- You can write string literals and use map functions inside of another string literal
- We use .join('') to remove the commas between the items in an array
```js
const dogs = [
    { name: "Snickers", age: 2 },
    { name: "Hugo", age: 8},
    { name: "Sunny", age: 1}
]

const markup = `
    <ul class="dogs">
        ${dogs.map(dog => `<li>${dog.name} is ${dog.age * 7}</li>`).join("")}
    </ul>    
`;

document.body.innerHTML = markup;
```

### Using if statements inside a string template using ternary operator
- This is taken from react.render() 
- We are writing a song name and artist
- If there is a featuring, we want to add that using a ternary operator
```js
const song = {
    name: "Dying to live",
    artist: "Tupac",
    featuring: "Biggie Smalls"
};

const markup = `
    <div class="song">
        <p>
            ${song.name} - ${song.artist}
            ${song.featuring ? `(Featuring ${song.featuring})` : ""}
        </p>
    </div>
`;
```

### Create a render function from react
- Make separate components to handle the complex information
- Here we are making a separate function to render all the keywords:
```js
const beer = {
    name: "Belgian Wit",
    brewery: "Steam Whistle Brewery",
    keywords: ["pale", "cloudy", "spiced", "crisp"]
};

// Our render function
function renderKeywords(keywords) {
    return `
        <ul>
            ${keywords.map(keyword => `<li>${keyword}</li>`).join("")}
        </ul>
    `;
}

// Our markup
const markup = `
    <div class="beer">
        <h2>${beer.name}</h2>
        <p class="brewery">${beer.brewery}</p>
        ${renderKeywords(beer.keywords)}
    </div>
`;
```
## Tagged Template Literals
- We can tag the strings
- We can used templates to craft our own strings
```js
const name = 'Snickers';
const age = 100;
const sentence = `My dogs's name is ${name} and he is ${age} years old`;

console.log(sentence);
```
- We tag the string with a function so it runs on the string
- Notice that we tagged the string with highlight word in front
- So the const sentence will be the result after the highlight function has processed the defined string
- It is basically a step in between
- Notice that we used logical operators to counter any undefined values
```js
function highlight(strings, ...values) {
    let str = '';
    strings.forEach((string, i) => {
        str += string + (values[i] || "");
    })
    return str;
}
const name = 'Snickers';
const age = 100;
const sentence = highlight`My dogs's name is ${name} and he is ${age} years old`;

console.log(sentence);
```
- Moving on, what can we do with this new string?
- We can highlight the values
- We can make boxes editable
```js
<style>
.hl {
    background: #ffc600;
}
</style>

function highlight(strings, ...values) {
    let str = '';
    strings.forEach((string, i) => {
        str += string + (values[i] || "");
        str += `${string} <span contenteditable class="hl">${values[i] || ""}</span>`;
    })
    return str;
}
const name = 'Snickers';
const age = 100;
const sentence = highlight`My dogs's name is ${name} and he is ${age} years old`;

console.log(sentence);
document.body.innerHTML = sentence;
```

### Sidenote #1 Rest Operator
- For the parameters, sometimes we don't know arguments already so we use the rest operator (...values) 
- The syntax: `function highlight(strings, names, age, etc.)` turns to `function highlight(strings, ...values)`

### Sidenote #2 Debugger with tagged templates
- After running the debugger:
```js
function highlight(strings, ...values) {
    debugger;
}

// Strings (Array[3]): 0:"My dog's name is", 1:" and he is ", 2:" years old"
// Values (Array[2]): 0:"Snickers", 1:100
```
- You will notice that the amount of strings is always 1 more than the values
- Strings has 3 values and Values has only 2 values
- It breaks ups the string to as many pieces as it can until a variable stops it

## Tagged Template Exercise
- When you're trying to add an abbreviation tag to a sentence
- Notice we do not have variables for HTML, CSS, JS
- But it will still render as a value which is useful later
- We also append the sentence with JS
- Setting the stage:
```js
<body>
    <div class="bio"></div>
</body>

const dict = {
    HTML: 'Hyper Text Markup Language',
    CSS: 'Cascading Style Sheets',
    JS: 'JavaScript'
}

const first = 'Wes';
const last = 'Bos';
const sentence = `Hello my name is ${first} ${last} and I love to code ${'HTML'}, ${'CSS'}, ${'JS'}`;

const bio = document.querySelector('.bio');
const p = document.createElement('p');
p.innerHTML = sentence;
bio.appendChild(p);
```

### Displaying abbreviations if it exists
- Now we create a function to return a new string
- The new string will show the abbreviations if exists
```js
<body>
    <div class="bio"></div>
</body>

const dict = {
    HTML: 'Hyper Text Markup Language',
    CSS: 'Cascading Style Sheets',
    JS: 'JavaScript'
}

function addAbbreviations(strings, ...values) {
    const abbreviated = values.map(value => {
        if(dict[value]) {
            return `<abbr title="${dict[value]}">${value}</abbr>`
        }
        return value;
    })
}

const first = 'Wes';
const last = 'Bos';
const sentence = `Hello my name is ${first} ${last} and I love to code ${'HTML'}, ${'CSS'}, ${'JS'}`;

const bio = document.querySelector('.bio');
const p = document.createElement('p');
p.innerHTML = sentence;
bio.appendChild(p);
```
### Stringing together the new sentence
- We can choose to use `let str = ''` or .reduce()
- 1. let str
```js
let str = '';
```
  
- 2. .reduce()
- Reduces takes 2 arguments (function, what you start with)
```js
<body>
    <div class="bio"></div>
</body>

const dict = {
    HTML: 'Hyper Text Markup Language',
    CSS: 'Cascading Style Sheets',
    JS: 'JavaScript'
}

function addAbbreviations(strings, ...values) {
    const abbreviated = values.map(value => {
        if(dict[value]) {
            return `<abbr title="${dict[value]}">${value}</abbr>`
        }
        return value;
    });

    return string.reduce((sentence, string, i) => {
        return `${sentence}${string}${abbreviated[i] || ''}`;
    }, '')
}

const first = 'Wes';
const last = 'Bos';
const sentence = `Hello my name is ${first} ${last} and I love to code ${'HTML'}, ${'CSS'}, ${'JS'}`;

const bio = document.querySelector('.bio');
const p = document.createElement('p');
p.innerHTML = sentence;
bio.appendChild(p);
```
## Sanitizing User Data with Tagged Templates
- With a security background, you need to sanitize the data
- They might insert JS that might be harmful to your website
- They might run some onload or JS to steal information, post as you
- Setting the stage:
```js
const first = 'Wes';
const aboutMe = `I love to do evil <img src="http://unsplash.it/100/100?random" onload="alert('you got hacked');" />`;

const html = `
    <h3>${first}</h3>
    <p>${aboutMe}</p>
`

const bio = document.querySelector('.bio');
bio.innerHTML = html;
```
  
- Now we are using [DOMpurify](https://github.com/cure53/DOMPurify) to add a script `<script type="text/javascript" src="dist/purify.min.js"></script>`
- The code to run it is `var clean = DOMPurify.sanitize(dirty);`
- We are also making a sanitize function:
```js
<script type="text/javascript" src="dist/purify.min.js"></script>

function sanitize(strings, ...values) {
    const dirty = strings.reduce((prev, next, i) => `${prev}${next}${values[i] || ''}`, '');
    return DOMpurify.sanitize(dirty);
}
const first = 'Wes';
const aboutMe = sanitize`I love to do evil <img src="http://unsplash.it/100/100?random" onload="alert('you got hacked');" />`;

const html = `
    <h3>${first}</h3>
    <p>${aboutMe}</p>
`

const bio = document.querySelector('.bio');
bio.innerHTML = html;
```