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
# Module #4 Additional String Improvements
## New String Methods
- Strings in ES6 comes with 4 new methods: `.startsWith()`, `.endsWith()`, `.includes()`, `.repeat()`
- Helps us decrease reliance on regex
- These methods are case-sensitive, there is not way to make it non-case sensitive (would need to use regex)
- Setting the stage:
```js
const course = 'RFB2';
const flightNumber = '20-AC2018-jz';
const accountNumber = '825242631RT0001';

const make = 'BMW';
const model = 'x5';
cons colour = 'Royal Blue';
```

### .startsWith()
- Results:
```js
course.startsWith('RFB'); // true
course.startsWith('rfb'); // false

// Can also start checking at a certain point
flightNumber.startsWith('AC'); // false
flightNumber.startsWith('AC', 3); // true, start checking after the 3rd character
```

### .endsWith()
- Results:
```js
flightNumber.endsWith('jz'); // true

accountNumber.endsWith('RT'); // false
accountNumber.endsWith('RT', 11); // true, take only the 1st eleven numbers into account
```
### .includes()
- Results:
```js
flightNumber.includes('AC'); // true
flightNumber.includes('ac'); // false
```

### .repeat()
- Allows you to repeat the strings how ever many times you want
- Results:
```js
// We are making a left-pad function
function leftPad(str, length = 20) {
    return `-> ${` `.repeate(length - str.length)}`;
}

console.log(leftPad(make));
console.log(leftPad(model));
console.log(leftPad(colour));

// Batman repeat string
`${'a' * 5}`.repeat(10) + ' Batman!';
```

# Module #5 Destructuring
## Destructuring Objects
- Destructuring allows us to extract data from objects, arrays, maps, and sets into their own variables
- Many time we just extract properties from the top level element which is inefficient
```js
const person = {
    first: 'Wes',
    last: 'Bos',
    country: 'Canada',
    city: 'Hamilton',
    twitter: '@wesbos'
};

const first = person.first;
const last = person.last;

// But now we can destructuring
const { first, last, twitter } = person;
```

### Deeper nested data
- With deeper data that we get from an API for example
```js
const wes = {
    first: 'Wes',
    last: 'Bos',
    links: {
        social: {
            twitter: 'https://twitter.com/wesbos',
            facebook: 'https://facebook.com/wesbos.developer',
        },
        web: {
            blog: 'https://wesbos.com'
        }
    }
}

const twitter = wes.links.social.twitter;
const facebook = wes.links.social.facebook;

// With destructuring:
// We are also renaming the variable as we are destructuring using:
// We are renaming variable names from twitter to tweet and facebook to fb
const { twitter:tweet, facebook:fb } = wes.links.social;
```

### Setting defaults
- Pretend we are making a function that does animations
- The function sets some settings for the animations
- We have width and color but it also needs height and fontSize
```js
var settings = {width: 300, color: 'black'}

// Require height and fontSize
// We destructure all four variables
const { width, height, color, fontSize} = settings;
```
- So when we destructure we can set a fallback also:
```js
var settings = {width: 300, color: 'black'}
const { width = 100, height = 100, color = 'blue', fontSize = 25 } = settings;

width; // 300
height; // 100
color; // "black"
fontSize; // 25
```

### Final example
- This is a combination of all the destructuring properties:
```js
const { w: width = 400, h: height = 500 } = {w: 800}

// Width will be 800 and height will be 500
// Notice the renaming of w to width and h to height also
```
## Destructuring Arrays
- Also works with arrays but we use brackets instead
```js
const details = ['Wes Bos', 123, 'wesbos.com'];

// Standard syntax:
const name = details[0];
const id = details[1];
const id = details[1];

// ES6 syntax;
const [name, id, website] = details;
console.log(name, id, website);
```

### Destructuring from a string
- 1st example:
```js
const data = 'Basketball,Sports,90210,23';
data.split(','); // ["Basketball", "Sports", "90210", "23"]

const [itemName, category, sku, iventory] = data.split(',');
console.log(itemName, category, sku, inventory); // Basketball Sports 90210 23
```

-2nd example:
```js
const team = ["Wes", "Harry", "Sarah", "Keegan", "Riker"];

const [captain, assistant, players] = team;
captain; // "Wes"
assistant; // "Harry"
players; // "Sarah" ???

// How do we get all the players?
// We use the rest operator, which looks similar to spread operator
const [captain, assistant, ...players] = team;
```

## Swapping Variables with Destructuring
- Wrestling example:
```js
// Standard syntax
let inRing = 'Hulk Hogan';
let onSide = 'The Rock';

inRing; // "Hulk Hogan"
inRing = onSide; // "The Rock"
inRing; // "The Rock"

// So we fix that with:
var temp = inRing;
inRing = onSide;
onSide = temp;
```
- Now we can switch variables with destructuring:
```js
// ES6 syntax
console.log(inRing, onSide); // Hulk Hogan The Rock
[inRing, onSide] = [onSide, inRing];
console.log(inRing, onSide); // The Rock Hulk Hogan
```

## Destructuring Functions - Multiple returns and named defaults
- Convert Currency example:
```js
function convertCurrency(amount) {
    const converted = {
        USD: amount * 0.76,
        GPD: amount * 0.53,
        AUD: amount * 1.01,
        MEX: amount * 13.30
    };
}

// Standard syntax
const hundo = convertCurrency(100);
console.log(hundo); // {USD: 76, GPB: 53, AUD: 101, MEX: 1330}
console.log(hundo.AUD); // 101
console.log(hundo.MEX); // 1330

// ES6 syntax
// The order does not matter since it's an object
const { USD, GPB, AUD, MEX } = convertCurrency(100);
console.log(USD, GPB, AUD, MEX ); // {76, 53, 101, 1330} 
console.log(USD, AUD); // Can choose and pick which to return

// Order does not matter
const { MEX, GPB, AUD, USD } = convertCurrency(100);
console.log(GPB, USD); 

MEX; // 1330
USD; // 76
```

### Named Arguments
- Here, we are destructuring the arguments as they are passed
- We are also setting default values
```js
function tipCalc({ total, tip = 0.15, tax = 0.13 }) {
    return total + (tip * total) + (tax * total);
}

// Default values kick in
const bill = tipCalc({ total: 200, tip: 0.20, tax: 0.13 });
console.log(bill); // 266

// If we leave an argument out, default values kick in
// Order of arguments does not matter
const bill = tipCalc({ tip: 0.20, total: 200});
```
  
- What if we ran it without arguments?
- We then have to destructure the whole arguments string
```js
// If you pass in nothing:
function tipCalc({ total, tip = 0.15, tax = 0.13 }) {
    return total + (tip * total) + (tax * total);
}
const bill = tipCalc(); // Error;

// So you have to destructure 
function tipCalc({ total = 100, tip = 0.15, tax = 0.13} = {}) {
    return total + (tip * total) + (tax * total);
}
```

# Module #6 Iterables & Looping
## The for of loop
- We now have the ForOf loop to loop over iterators
- Iterators are things that you can loop over
- Like arrays, strings, maps, sets, generators

### For Loop
- A For loop is confusing, hard to read
```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

for (let i  0; i < cuts.length; i++) {
    console.log(cuts[i]); // Chuck Brisket Shank Short Rib
}
```

### ForEach loop
- Can't abort loop or skip an entry
- Using break won't work, will return error
- Using continue won't work, will return error
```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

cuts.forEach((cut) => {
    console.log(cut); // Chuck Brisket Shank Short Rib
});
```

### ForIn loop
- Returns index for us
```js
for (const index in cuts) {
    console.log(cuts[index]); // Chuck Brisket Shank Short Rib
}
```
- The problem is that if the prototype is altered
- There are libraries (ie MooTools) and people that change array.prototype
- Wes's Shuffle prototype for example:
- So it shuffles an array for us
```js
Array.prototype.shuffle = function() {
    var i = this.length, j, temp;
    if (i == 0) return this;
    while (--i) {
        j = Math.floor(Math.random() * (i+1));
        temp = this[i];
        this[i] = this[j];
        this[j] = temp;
    }
    return this;
};
```
- But when we run the ForIn function:
- It iterates over the function also, which we don't want
```js
for (const index in cuts) {
    console.log(cuts[index]); // Chuck Brisket Shank Short Rib function(...)
}
```
  
- Another example:
```js
var names = ['wes', 'lux']
for(name in names) { console.log(name)}; // 0, 1, and a ton of methods
```

### ForOf loop
- Need to put const in or it will leak into the global variable
- ForOf loops can only handle iterables, it can't do objects
```js
for (const cut of cuts) {
    console.log(cut); // Chuck Brisket Shank Short Rib
}
```
- If we want to put a break after Brisket:
```js
for (const cut of cuts) {
    console.log(cut); // Chuck Brisket
    if(cut === 'Brisket') {
        break;
    }
}
```
- If we want to skip Brisket:
```js
for (const cut of cuts) {
    console.log(cut); // Chuck Shank Short Rib
    if(cut === 'Brisket') {
        continue;
    }
}
```

## The for of Loop in Action
- Can generally handle all types of datas except objects
```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

for(const cut of cuts) {
    console.log(cut);
}
```
- We can use entries:
- Put this into the console to experiment
```js
const meat = cuts.entries(); // ArrayIterator

meat.next(); // Returns the next object in the array
```
- So we update our code:
```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

for(const cut of cuts.entries()) {
    console.log(cut); // We get arrays back
}

// Instead, we can destructure:
for(const [i, cut] of cuts.entries()) {
    console.log(`${cut} is the ${i+1} item`); // Chuck is the 1 item, Brisket is the 2 item
}
```

### Iterating over the arguments objects
- Add up numbers example:
- "arguments" looks exact like a regular array and has only length property
```js
function addUpNumbers() {
    console.log(arguments); // Returns an arrayish, not an exact array
}

addUpNumbers(10,23,52,34,12,13,123);
```
- Normally, you can just make an array out of the "arguments", then .reduce() it
- There's another way to convert without using an array:
```js
function addUpNumbers() {
    let total = 0;
    for (const num of arguments) {
        total += num;
    }
    console.log(total);
    return total;
}

addUpNumbers(10,23,52,34,12,13,123); // 267
addUpNumbers(10,10); // 10
addUpNumbers(10,10,2,34,2342,34,23,42,34,2); // 256
```

### Iterating over a string
- You can loop over strings and also DOM collections (more later)
```js
const name = 'Wes Bos';
for (const char of name) {
    console.log(char); // W e s B o s
}
```
### DOM collectors 
- Node list, or html collections are being changed but not true arrays in most browsers
```html
<!--HTML-->
<p>Hi I'm p 01</p>
<p>Hi I'm p 02</p>
<p>Hi I'm p 03</p>
<p>Hi I'm p 04</p>
<p>Hi I'm p 05</p>
<p>Hi I'm p 06</p>
<p>Hi I'm p 07</p>
<p>Hi I'm p 08</p>
<p>Hi I'm p 09</p>
<p>Hi I'm p 10</p>
```
```js
// JS
const ps = document.querySelectorAll('p');
console.log(ps); // Get a node list but does have forEach method

for (const paragraph of ps) {
    console.log(paragraph);

    paragraph.addeventListener('click', function() {
        console.log(this.textContent);
    })
}

```

## Using for of with Objects
- We have not been able to iterate over objects
- There is no [Symbol.iterator]
- We can use `.entries()` as in `for (const prop of apple.entries()) {}`
- Object.values() and Object.entries() are going to be in ES2017, for now we can polyfill
- But now we can use Object.keys() for now, it returns the keys of an object
- Wes's recommendation is to use a ForIn until we get .entries() for objects
```js
const apple = {
    color: 'Red',
    size: 'Medium',
    weight: 50,
    sugar: 10
}

// ForOf
for (const prop of Object.keys(apple)) {
    const value = apple[prop];
    console.log(value, prop); // Red color Medium size 50 "Weight" 10 "sugar"
}

// ForIn
for (const prop in apple) {
    const value = apple[prop]];
    console.log(value, prop); // Red color Medium size 50 "Weight" 10 "sugar"
}
```

# Module #7 An Array of Array Improvements
## Array.from() and Array.of()
- These methods are not on the prototype, they are on the array itself
- Array.from() will turn something into an array from something that is array-ish
- People example:
```html
<div class="people">
    <p>Wes</p>
    <p>Kait</p>
    <p>Snickers</p>
</div>
```
```js
const people = document.querySelectorAll('.people p');
const names = people.map(person => person.textContent);

console.log(people); // Nodelist
```
- Since it is a Nodelist, we need to convert it
```html
<div class="people">
    <p>Wes</p>
    <p>Kait</p>
    <p>Snickers</p>
</div>
```
```js
// Method 1
// We make an array from the DOM elements
const people = document.querySelectorAll('.people p');
const peopleArray = Array.from(people);
const names = peopleArray.map(person => person.textContent);
console.log(names);

// Method 1 refactor
const people = Array.from(document.querySelectorAll('.people p');)
const names = people.map(person => person.textContent);
console.log(names);

// Method 2
// Array.from has a built in map function
const people = document.querySelectorAll('.people p');
const peopleArray = Array.from(people, person => {return person.textContent});
console.log(peopleArray);
```

### Converting arguments object to an array
- Common use case because "arguments" reserve keyword is not a real array
```js
function sumAll() {
    console.log(arguments); // Arrayish, not an array
    arguments.reduce((prev, next) => prev + next, 0);
}

sumAll(2,34,23,234,234,234234,234234,2342);

// We need to convert to a real array
function sumAll() {
    const nums = Array.from(arguments);
    arguments.reduce((prev, next) => prev + next, 0);
}
```

### Array.of() method
- When we need to convert arguments into an array:
- It could be numbers or strings
- You have to capitalize the A in array or will not work
- Different from Array() method because that will return an empty array with length property if supplied a single digit
```js
const ages = Array.of(12,4,23,62,34);
console.log(ages); // returns an array of these numbers
```

## Array.find() and .findIndex()
- Helps you to find some data that comes back from an API
- .find() helps you find if something exist, returns boolean
- .findIndex() finds the index where something occurs
- Twitter example;
```js
// Will now return that specific instagram post from that ID
// To find multiple objects, use .filter()
const post = posts.find(post => {
    console.log(post.code);
    if(post.code === 'VBgtGQcSf') {
        return true;
    }
    return false;
})

// Refactors to:
const code = 'VBgtGQcSf';
const post = posts.find(post => post.code === code);
console.log(post);
```

### .findIndex()
```js
const postIndex = posts.findIndex((post) => {
    if(post.code === code) {
        return true;
    }
    return false;
})

// Refactors to:
const postIndex =  posts.findIndex(post => post.code === code);
```

## Array.some() and .every()
- Not part of ES6 but want to showcase it
- .some() checks if at least one meets the requirement
- .every() checks if all meets the requirement
```js
const ages = [32, 15, 19, 12];
const youngins = [1, 2, 2, 5];

// Is there at least one adult in the group? (>18 years old)
const adultPresent = ages.some(age => age >= 18);
console.log(adultPresent); // true
console.log(youngins); // false

// Is everyone old enough to drink? (>19 years old)
const allOldEnough = ages.every(age => age >= 19);
console.log(allOldEnough); // false
```

# Module #8 Say Hello to ...Spread and ...Rest
## Spread Operator Introduction
- Takes every item from an iterable and returns each
- In this example, we are trying to add veg to the middle of the 2 arryas
```js
// Standard syntax
const featured = ['Deep Dish', 'Pepperoni', 'Hawaiian'];
const specialty = ['Meatzza', 'Spicy Mama', 'Margherita'];

let pizzas = pizzas.concat(featured);
pizzas.push('veg');
pizzas = pizzas.concat(specialty);
consoel.log(pizzas);

// ES6 syntax
const pizzas = [...featured, ...specialty];
    // ['Deep Dish', 'Pepperoni', 'Hawaiian', 'Meatzza', 'Spicy Mama', 'Margherita'];
const pizzas = [...featured, 'veg', ...specialty];
    // ['Deep Dish', 'Pepperoni', 'Hawaiian', 'veg', 'Meatzza', 'Spicy Mama', 'Margherita'];
``
- If you use spread on a string:
- It goes over each character instead
```js
['wes'] // ["Wes"]
[...'wes'] // ["w", "e", "s"]
```

### Copying an array
```js
// Standard syntax
const pizzas = [...featured, 'veg', ...specialty];
    // ['Deep Dish', 'Pepperoni', 'Hawaiian', 'veg', 'Meatzza', 'Spicy Mama', 'Margherita'];
const fridayPizzas = pizzas;
    // ['Deep Dish', 'Pepperoni', 'Hawaiian', 'veg', 'Meatzza', 'Spicy Mama', 'Margherita'];

fridayPizzas[0] = "Canada Pie";
fridayPizzas;
    // ['Canada Pie', 'Pepperoni', 'Hawaiian', 'veg', 'Meatzza', 'Spicy Mama', 'Margherita'];
pizzas;
    // ['Canada Pie', 'Pepperoni', 'Hawaiian', 'veg', 'Meatzza', 'Spicy Mama', 'Margherita'];

// But pizzas array also changed which we do not want
// We simply reference the array, not make a copy
// We use to use concat:
const fridayPizas = [].concat.pizzas;

// ES6 syntax
const fridayPizzas = [...pizzas];
fridayPizzas[0] = "Canada Pie";

fridayPizzas;
    // ['Canada Pie', 'Pepperoni', 'Hawaiian', 'veg', 'Meatzza', 'Spicy Mama', 'Margherita'];
pizzas;
    // ['Deep Dish', 'Pepperoni', 'Hawaiian', 'veg', 'Meatzza', 'Spicy Mama', 'Margherita'];
```

## Spread Exercise
- We are spreading a string, and each letter will animate
- You can hover over each letter so you have to wrap in a span
- Use map, spread, querySelectorAll

## More Spread Examples
- Another example when we want to return from a DOM node:
```html
<div class="people">
    <p>Wes</p>
    <p>Kait</p>
    <p>Randy</p>
</div>
```
```js
const people = document.querySelectorAll(".people p");
const names = people.map((person) => person.textContent);

// Does not work because people is a node list, not an array
```
- So we need to use 1 of 2 ways:
- Wes thinks Array.from() reads better
```js
// Method 1: Array.from
const people = Array.from(document.querySelectorAll(".people p"));
const names = (people.map((person) => person.textContent));

//Method 2 : Spread operator
const people = [...document.querySelectorAll(".people p")];
const names = (people.map((person) => person.textContent));
```

### New array off a property of an object
- The spread is a true copy and not a reference
```js
const deepDish = {
    pizzaName: 'Deep Dish',
    size: 'Medium',
    ingredients: ['Marinara', 'Italian Sausage', 'Dough', 'Cheese']
}

// So we add the ingredients to our shopping list to buy
const shoppingList = ['Milk', 'Flour', ...deepDish.ingredients];
```

### Deleting an object off the array
- He wants to remove index 2 comment
```js
const comments = [
    { id: 209384, text: 'I love your dog!' },
    { id: 523423, text: 'Cuuute! 🐐' },
    { id: 632429, text: 'You are so dumb' },
    { id: 192834, text: 'Nice work on this wes!' },
];

// We have the id of the commment
const id = 632429;
const commentIndex = comments.findIndex(comment => comment.id === id);

// We know now it's at index 2, so we are going to make a new comment variable
const newComments = [comments.slice(0, commentIndex), comments.slice(commentIndex + 1)];

// But we get an array of arrays, so we have to use the spread operator
const newComments = [...comments.slice(0, commentIndex), ...comments.slice(commentIndex + 1)];
```
## Spreading into a function
- We want to tack values from an array onto another array
- Inventors example:
```js
const inventors = ['Einstein', 'Newton', 'Galileo'];
const newInventors = ['Musk', 'Jobs'];

// Not our target
inventors.push(newInventors) // ['Einstein', 'Newton', 'Galileo', Array[2]]

// Standard syntax
// It pushes each item into an array instead of the whole array
inventors.push.apply(inventors, newInventors);
console.log(inventors) = ['Einstein', 'Newton', 'Galileo', 'Musk', 'Jobs']

// ES6 syntax
inventors.push(...newInventors);
console.log(inventors) = ['Einstein', 'Newton', 'Galileo', 'Musk', 'Jobs']
```
  
### Spread in a function
```js
const name = ['Wes', 'Bos'];

function sayHi(first, last) {
    alert(`Hey there ${first} ${last}`);
}

// Standard syntax
sayHi(name[0], name[1]); 

// ES6 syntax
sayHi(...name);
```

## The ...rest param in Functions and destructuring
- Rest is the opposite of Spread
- Rest packs items into an array
```js
function convertCurrency(rate, amount1, amount2, amount3, amount4)
convertCurrency(1.54, 10, 23, 52, 1, 56);

// ES6 syntax
function convertCurrency(rate, ...amounts) {
    console.log(rate, amounts);
}
convertCurrency(1.54, 10, 23, 52, 1, 56); // 1.54 > [10, 23, 52, 1, 56]
convertCurrency(1.54, 10); // 1.54 > [10]
```
- Then we want to take the amounts and multiply by the rate:
```js 
function convertCurrency(rate, ...amounts) {
    return amounts.map(amount => amount * rate);
}

const amounts = convertCurrency(1.54, 10, 23, 52, 1, 56);
console.log(amounts); // [15.4, 35.42, 80.08, 1.5, 86.24]
```
- We can also use as many pre-arguments also:
```js 
function convertCurrency(rate, tax, tip ...amounts) {
    return amounts.map(amount => amount * rate);
}

const amounts = convertCurrency(1.54, 10, 23, 52, 1, 56);
console.log(amounts); // [15 10 23 > [52, 1, 56]]
```
  
- Runner example:
```js
cons runner = ['Wes Bos', 123, 5.5, 5, 3, 6, 35];
const [name, id, runs] = runner;
console.log(name, id, runs); // Wes Bos 123 5.5

// But we can get the rest of the stats using the rest param:
cons runner = ['Wes Bos', 123, 5.5, 5, 3, 6, 35];
const [name, id, ...runs] = runner;
console.log(name, id, runs); // Wes Bos 123 > [5.5, 5, 3, 6, 35]
```

- Team example:
```js
const team = ['Wes', 'Kait', 'Lux', 'Sheena', 'Kelly'];
const [captain, assistant, ...players] = team;
console.log(captain, assitant, players); // Wes Kait > ["Lux", "Sheena", "Kelly"]
```

# Module #9 Object Literal Upgrades
## Object Literal Upgrades
- When you need to put variables into an object:
```js
const first = 'snickers';
const last = 'bos';
const age = 2;
const breed = 'King Charles Cav';

// Standard syntax
const dog = {
    first: first,
    last: last,
    age: age,
    breed: breed,
};
console.log(dog); // Object{first: "snickers", last: "bos", age: 2, breed: "King Charles Cav"}

// ES6 syntax
// If you have the same value names as key names, you can just:
const dog = {
    first,
    last,
    age,
    breed,
};
console.log(dog); // Object{first: "snickers", last: "bos", age: 2, breed: "King Charles Cav"}
```

- You can also change or add the name of the keys if you want:
```js
const dog = {
    firstName: first,
    last,
    age,
    breed,
    pals: ['Hugo', 'Sunny']
};
console.log(dog); // Object{firstName: "snickers", last: "bos", age: 2, breed: "King Charles Cav", pals: Array[2]}
``` 

### When we have methods inside of an object
- Modal example:
```js
// Standard syntax
const modal = {
    create: function() {

    },
    open: function() {

    },
    close: function() {

    }
}

// ES6 syntax
// You should not use arrow method with methods of a object
// You can add arguments as normal
const modal = {
    create(selector) {

    },
    open(content) {

    },
    close(goodbye) {

    }
}
```

### Computed property names
- When we need to set a key on an object
- T-Shirt example:
```js
// Standard syntax
const key = 'pocketColor';
const value = '#ffc600';

const tShirt = {
    [key]: value
}

console.log(tShirt); // Object{pocketColor: "#ffc600"}

// ES6 syntax
const key = 'pocketColor';
const value = '#ffc600';

function invertColor(color) {};
const tShirt = {
    [key]: value,
    [`${key}Opposite`]: invertColor(value);
};

console.log(tShirt); // Object{pocketColor: "#ffc600"}

// In the past, you had to make a tShirt object and update that object
const tShirt = {};

tShirt[key]: value,
tShirt[`${key}Opposite`]: invertColor(value)
```

### Pulling values from both arrays
- We want to pair values in both arrays into an object
- Shirt spec example:
```js
const keys = ['size', 'color', 'weight'];
const values = ['medium', 'red', 100];

// Standard syntax
const shirt = {
   // [keys[0]] take out brackets
}

// ES6 syntax
const shirt = {
    [keys.shift()]: values.shift(),
    [keys.shift()]: values.shift(),
    [keys.shift()]: values.shift(),
}
// Object {size: "medium", color: "red", weight: "100"}
```

# Module #10 Promises
## Promises
- Often used when you're using jQuery ($.getJSON) or AJAX ($.ajax)
- Here, Wes is using fetch API which is built into the browser
- Definition: Something that happens in the future and is asynchronous
- Fetch queues up and gives you a promise (fetch(''))
- When the response returns, it will deliver the results 
- We can set up listeners (.then())
- Here's a basic setup:
```js
const postsPromise = fetch('http://wesbos.com/wp-json/wp/v2/posts');

// Fetch API
postsPromise.then(data => {
    console.log(data);
})

// Just like jQuery
$('a').on('click', function() {
    alert('hey');
})
```
- But where is our data?
- Doesn't know to return as json so we have to set it:
- We also set .catch() in case of any errors along the way
```js
const postsPromise = fetch('http://wesbos.com/wp-json/wp/v2/posts');

// Fetch API
postsPromise
    .then(data => data.json())
    .then(data => {console.log(data)})
    .catch((err) => {console.error(err)})
})
```

## Building your own Promises
- We can set resolve and reject parameters
- We can even set a timeout
- Set a throw error as well to help debug which line
```js
const p = new Promise((resolve, reject)) => {
    setTimeout(() => {
        resolve('Wes is cool!');
    }, 1000);
    reject(Error('Err wes isn\'t cool'));
});

p
    .then(data => {
        console.log(data);
    })
    .catch(err => {
        console.error(err);
    })
```
## Chaining Promises + Flow Control
- Let's simulate a database where we are trying to pull posts and authors
- So we have to fetch promises in practice
- Here, we are creating a new promise
- Find the post that we want
- Send the post back
```js
const posts = [
    { title: 'I love JavaScript', author: 'Wes Bos', id: 1 },
    { title: 'CSS!', author: 'Chris Coyier', id: 2 },
    { title: 'Dev tools tricks', author: 'Addy Osmani', id: 3 },
];

const authors = [
    { name: 'Wes Bos', twitter: '@wesbos', bio: 'Canadian Developer' },
    { name: 'Chris Coyier', twitter: '@chriscoyier', bio: 'CSS Tricks and CodePen' },
    { name: 'Addy Osmani', twitter: '@addyosmani', bio: 'Googler' },
];

function getPostById(id) {
    return new Promise((resolve, reject) => {
        const post = posts.find(post => post.id === id);
        if(post) {
            resolve(post);
        } else {
            reject(Error('No Post Was Found'));
        }
    })
}

getPostById(2)
    .then(post => {
        console.log(post)
    });
```

### Hydrating
- We want to replace the author string in posts with authors object
- We create a new promise
- We find the author
- "Hydrate" the post object with the author object
```js
const posts = [
    { title: 'I love JavaScript', author: 'Wes Bos', id: 1 },
    { title: 'CSS!', author: 'Chris Coyier', id: 2 },
    { title: 'Dev tools tricks', author: 'Addy Osmani', id: 3 },
];

const authors = [
    { name: 'Wes Bos', twitter: '@wesbos', bio: 'Canadian Developer' },
    { name: 'Chris Coyier', twitter: '@chriscoyier', bio: 'CSS Tricks and CodePen' },
    { name: 'Addy Osmani', twitter: '@addyosmani', bio: 'Googler' },
];

function getPostById(id) {
    return new Promise((resolve, reject) => {
        const post = posts.find(post => post.id === id);
        if(post) {
            resolve(post);
        } else {
            reject(Error('No Post Was Found'));
        }
    })
}

function hydrateAuthor(post) {
    return new Promise((resolve, reject) => {
        const authorDetails = authors.find(person => person.name === post.author)
        if(authorDetails) {
            post.author = authorDetails;
            resolve(post);
        } else {
            reject(Error('Cannot find the author'));
        }
    });
}

getPostById(2)
    .then(post => {
        console.log(post);
        return hydrateAuthor(post);
    })
    .then(post => {
        console.log(post);
    })
    .catch(err => {
        console.error(err);
    })

getPostById(2) // { name: 'Chris Coyier', twitter: '@chriscoyier', bio: 'CSS Tricks and CodePen' }
getPostById(5) // No Post Was Found!
getPostById(3) // Cannot find the author if the author's name is mispelled
```

## Working with Multiple Promises
- Now, we want to fetch multiple promises
- We are trying to get the weather and tweets
- We can use Promise.all() for this purpose
- Notice that the slowest time is the rate limiting step (ie weather 2 secs, tweets 0.5 sec)
```js
const weather = new Promise((resolve) => {
    setTimeout(() => {
        resolve({ temp: 29, conditions: 'Sunny with Clouds'});
    }, 2000);
});

const tweets = new Promise((resolve) => {
    setTimeout(() => {
        resolve(['I like cake', 'BBQ is good too!']);
    }, 500);
});

Promise
    .all([weather, tweets])
    .then(responses => {
        const [weatherInfo, tweetsInfo] = responses;
        console.log(weatherInfo, tweetsInfo)
    })
```

### Using real data
- With fetch API, you need to be running it through some sort of server
- It's a CORS issue
- Wes has a browser-sync plugin
- We have to map the response to JSON beccause there can be multiple returns:
1. arrayBuffer()
2. blob()
3. json()
4. text()
5. formData()
```js
const postsPromise = fetch('http://wesbos.com/wp-json/wp/v2/posts');
const streetCarsPromise = fetch('http://data.ratp.fr/api/datasets/1.0/search/?q=paris');

Promise
    .all([postsPromise, streetCarsPromise])
    .then(responses => {
        return Promise.all(responses.map(res => res.json()))
    })
    .then(responses => {
        console.log(responses);
    });
```