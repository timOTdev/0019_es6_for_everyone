# ES6 For Everyone 
## By Wes Bos
Started 10-08-2017 Finished 11-3-2017

# Getting Setup
- Get the latest browser which is currently most browsers
- Wes is using Chrome in this course

- Download the starter files
- Feel free to download video files to watch offline if you prefer
- data.js has random-generated data to use in the course

# Module #1 New Variables - Creation, Updating and Scoping
## var Scoping Refresher
- We have 3 ways to declare JS variables:
1. `var`
- There's no problem if we put var on the 2nd width also
- We can use `var` to update the variable:
```js
var width = 100;
console.log(width); // 100
width = 200;
console.log(width); // 200
```
  
- `var` variables are function-scoped
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
console.log(width); // Will work because the value is pushed outside of the function
```

- Variables are generally scoped in functions, but not scoped with if statements
- We just needed dogYears variable temporarily here but it leaked to the global window:
```js
var age = 100;
if(age > 12) {
    var dogYears = age * 7;
    console.log(`you are ${dogYears} dog years old!`);
}
console.log(dogYears); // Leaked outside of the if block to the global window
```
2. `let`
- Let is scoped to a block, one of its attributes
```js
var age = 100;
if(age > 12) {
    let dogYears = age * 7;
    console.log(`you are ${dogYears} dog years old!`);
}
console.log(dogYears); // error
```

3. `const`
- Const is scoped to a block, one of its attributes
```js
var age = 100;
if(age > 12) {
    const dogYears = age * 7;
    console.log(`you are ${dogYears} dog years old!`);
}
console.log(dogYears); // error
```

## let VS const
1. `Let` can only be declared once inside of a scope (ie a code block)
```js
let points = 50;

let points = 60; // error
points = 60; // However, it can be updated
```

2. `Let` can have the same variable name but can be scoped differently
- The first `let` is scoped to the window object
- The second `let` is scoped to the if statement block
- If you change winner variable to `var`, it will return true
```js
let winner = false;
let points = 45;

if (points > 40) {
    let winner = true;
}

winner; // false
```

3. `Const` variables cannot be changed but the properties can be updated
- It can only ever be defined once in the whole scope of the file
```js
const key = "abc123";

key = "abc1234"; // error
```
  
- But the properties of the variable inside of the object can be changed
- A good way to think about it is the person will never change
- But their attributes can change like their age, hair, clothes
```js
const person = {
    name: "Tim",
    age: "99"
}

person = {name: "Timothy"} // error, can't change the variable
person.age = 100; // can change the properties of a variable
person.age; // 100
```

- Another important concept if you want to permanently freeze a variable is to use `Object.freeze()`
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

## `let` and `const` in the Real World
1. `Let` and `const` replaces the IFFE (Immediately-Invoked Function Expression)
```js
var name = "Tim"

name; // "Tim"
```

- IFFEs are self-contained so that nothing was leaked into the parent scope
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

- Also, on the window object, there is already a natively defined property called `name` that refers to current window you're on
- So that's why you get a "" when you call name on the window object
  
- Using `let` and `const` solves this leaking to parent scope problem also:
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
  
2. `Let` solves the problem with for loops
- Setting the stage:
```js
for (var i = 0; i < 10; i ++) {
    console.log(i);
}
// 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
i; // 10
```

- If we run a `setTimout()` method:
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

- Notice that the results of the `for` loop completes before 1 second and then runs the `console.log()` afterwards
- At this point, `i` is already 10 since it completed outside of the `function()`
- The reason for this is because var is function scoped
- So if you want to display "The number is i" with i incrementing in value, use `let` 
- Remember `let` is block scoped, so it will execute 10 instances of whats in the block
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

- But what if you call `console.log(pizza)` before the variable?
```js
console.log(pizza); // You get undefined
var pizza = "Deep Dish";

// You can access the variable pizza but not the value
// Basically, the creation of the variable is acknowledged and you get undefined
// You just can't get the value of the variable
// To access the value, you have to declare the variable at the top
```
  
- With `const` or `let` variables, you will get an error:
- Neither have been pre-declared beforehand
```js
console.log(pizza); // error
let pizza = "Deep Dish";

console.log(pizza); // error
const pizza = "Deep Dish";
```

## Is var Dead? What should I use?
- Two popular opinions exists for the usage of `var`, `let`, and `const`
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

// 2nd pass: Remove the parentheses on the parameter
// You can choose to use them with one argument which is okay too
// If you have multiple arguments, you should use the parentheses
const fullNames3 = names.map(name => {
    return `${name} bos`;
})

// 3rd pass: Create an implicit return
// Remove the return and curly brackets
const fullNames4 = names.map(name => `${name} bos`);

// 4th pass: Remove argument if there are none
// Still need empty parentheses
const fullNames5 = names.map(() => `cool bos`);
```

### Arrow functions are anonymous functions
- Unlike a named function, you can't give the arrow function a name:
```js
// You can't do this with arrow functions
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
- Notice that we used 2 parameters, must have parentheses
- If we want to return an object implicitly, must have parentheses around that object
- Otherwise, it is treated as brackets of a function
- We also have to add +1 on to i because it's based off 1st, 2nd, 3rd place and not 0
- Also, use console.table(), it's cool
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
- Normally we use an `if` statement, but we can use .filter() instead
- It looks strange with `=>` and `>=` in the same line:
```js
const ages = [23,62,45,234,2,62,234,62,34];

const old = ages.filter(age => age >= 60);
console.log(old); // [62,234,62,234,62]
```

## Arrow Functions and `this`
- When using arrow functions, the `this` keyword does not get rebound
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
// The arrow function is not inside of the another block either
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
box.addEventListener('click', function() => { // Reference B: "this" from following line refers to box
    this.classList.toggle('opening'); // Reference A: setTimeout's "this" refers here, "this" refers to Reference B
    setTimeout(() => {
        console.log(this); // "this" refers to the Reference A
        this.classList.toggle('open');
        }, 500);
    });

// Arrow functions do not change the value of "this"
// It inherits the value of "this" from the parent function
// Reason is because it is inside of another function
// We do not have to worry about the scope changing
// It will now take on the value of "this" in `this.classList`, which in turn refers to the box 
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
- Default function arguments make your code much more readable and maintainable
- Setting the stage for a calculate bill example:
```js
function calculateBill(total, tax, tip) {
    return total + (total * tax) + (total * tip);
}

const totalBill = calculateBill(100, 0.13, 0.15);
console.log(totalBill); // 128

const totalBill = calculateBill(100);
console.log(totalBill); // NaN
// So how we set a default for the tax and tip?
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
- Instead now in ES6, we can just set the values as default in the parameters:
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
console.log(totalBill); // error
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
// "this" was bound to the window object because it was not inside of a function
// If you console.log(this), you will find that it's the window
const button = document.querySelector('#pushy');
button.addEventListener('click', () => {
    this.classList.toggle('on');
});

// The right way
// "this" will be correctly bound to the button instead of the window
const button = document.querySelector('#pushy');
button.addEventListener('click', function() {
    this.classList.toggle('on');
});
```
  
### 2. When you need a method to bind to an object:
```js
// The wrong way
// "this" keyword was not bound to the object so you need to use function()
// If you console.log(this), you will find that it's the window
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
- We have not learned about ES6's Class yet
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
    return `this car is a ${this.make} in the colour ${this.colour}`;
}

subie; // Car {make: "Subaru", colour: "white"}
beemer; // Car {make: "bmw", colour, "blue"}
subie.summarize(); // "This car is undefined in the colour undefined"
beemer.summarize(); // "This car is undefined in the colour undefined"
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
// You do not get arguments if you use an arrow function
const orderChildren = () => {
    const children = Array.from(arguments);
    return children.map((child, i) => {
        return `${child} was child #${i + 1}`;
    })
    console.log(arguments); // Error;
}

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
- Now in JS, it's possible with template strings (aka template literals)
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
- In the past, you had to add backslashes:
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

const markup =`<ul class="dogs">
        ${dogs.map(dog => `<li>${dog.name} is ${dog.age * 7}</li>`).join("")};
    </ul>`;

document.body.innerHTML = markup;
```

### Using if statements inside a string template using ternary operator
- This is taken from React.render() 
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

### Create a render function from React
- Make separate components to handle the complex information in React
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
- Notice that we tagged the string with `highlight` in front
- So the const sentence will be the result after the highlight function has processed the defined string
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
const sentence = highlight`My dog's name is ${name} and he is ${age} years old`;

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
- The syntax: `function highlight(strings, names, age, etc.)` can be shortened to `function highlight(strings, ...values)`

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
- `strings` has 3 values and `values` has only 2 values
- It breaks ups the string to as many pieces as it can until a variable stops it

## Tagged Template Exercise
- When you're trying to add an abbreviation tag to a sentence
- Notice we do not have variables for HTML, CSS, JS
- But it will still render as a value if placed inside template strings
- We also append the sentence with JS with `.appendChild()`
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

    return strings.reduce((sentence, string, i) => {
        return `${sentence}${string}${abbreviated[i] || ''}`;
    }, '')
}

const first = 'Wes';
const last = 'Bos';
const sentence = addAbbreviations`Hello my name is ${first} ${last} and I love to code ${'HTML'}, ${'CSS'}, ${'JS'}`;

const bio = document.querySelector('.bio');
const p = document.createElement('p');
p.innerHTML = sentence;
bio.appendChild(p);
```

## Sanitizing User Data with Tagged Templates
- With security issues, you need to sanitize the data
- Users might insert JS during a prompt that might be harmful to your website
- They might run some onload, steal information, or post as you on facebook
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
  
- Now we are using [DOMpurify](https://github.com/cure53/DOMPurify)
- Add a script to the bottom of the body `<script type="text/javascript" src="dist/purify.min.js"></script>`
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
- Now the code should be clean
- You can tag on the sanitize function anywhere you need it

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
const colour = 'Royal Blue';
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
    return `-> ${` `.repeat(length - str.length)}`;
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
// We can also renaming the variable as we are destructuring using:
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
- So when we destructure we can set a default also:
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
- Also works with arrays and we use brackets instead
```js
const details = ['Wes Bos', 123, 'wesbos.com'];

// Standard syntax:
const name = details[0];
const id = details[1];
const id = details[1];

// ES6 syntax:
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

- 2nd example:
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

// So the old syntax we fix that with:
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
const { USD, GPB, AUD, MEX } = convertCurrency(100);
console.log(USD, GPB, AUD, MEX); // {76, 53, 101, 1330} 
console.log(USD, AUD); // Can choose and pick which to return

// Order does not matter or if we leave something off
const { MEX, GPB, AUD, USD } = convertCurrency(100);
const { GPB, USD } = convertCurrency(100);
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
const bill = tipCalc({ tip: 0.20, total: 200 });
```
  
- What if we ran it without any arguments?
- We then have to destructure the whole arguments string
```js
// If you pass in nothing:
function tipCalc({ total, tip = 0.15, tax = 0.13 }) {
    return total + (tip * total) + (tax * total);
}
const bill = tipCalc(); // Error;

// So you have to destructure the whole object
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

# Module #11 Symbols
## All About Symbols
- We have a 7th type of primitive now: symbols
- Helps us to avoid naming collisions
- They are absolutely unique
```js
// Think of Wes here as a descriptor
const wes = Symbol('Wes'); // Think of it as a unique symbol A3awD21Qhd8kdqpx
const person = Symbol('Wes')

wes; // Symbol(Wes)
person; // Symbol(Wes)
wes === person; // false
wes == person; // false
```
### Classroom example
- Here, we have two Olivia's but we don't want them to collide
- This is a small example but think of bigger applications like graphics or 
svg
- Symbols are not innumerable so you can't loop through them
```js
const classRoom = {
    'Mark': { grade: 50, gender: 'male' },
    'Olivia': { grade: 80, gender: 'female' },
    'Olivia': { grade: 70, gender: 'female' },
}
```
- We can use symbols here:
```js
const classRoom = {
    [Symbol('Mark')]: { grade: 50, gender: 'male' },
    [Symbol('Olivia')]: { grade: 80, gender: 'female' },
    [Symbol('Olivia')]: { grade: 70, gender: 'female' },
}

// Symbols are innumerable and can't loop over them
// Good way to store private data
// This for loop will not work
for (person in classRoom) {
    console.log(person)
}
```
- We can use a property to access the object
```js
const syms = Object.getOwnPropertySymbols(classRoom);
console.log(syms); // Returns an array of the key of the symbol

// To get the data
const data = syms.map(map => classRoom[sym]);
console.log(data); // Returns an array of objects
```

# Module #12 Code Quality with ESLint
## Getting started with ESLint
- Most people have moves over to ESlint from JSLint or JSHint
- Configure it properly and it will show you errors
- It is more updated for ES6
- Takes an hour or 2 to adjust to it
- Helps improve your code
- Need nodeJS and NPM install
- Run:
```
node -v // For node version, 4 and up
npm -v // For npm version, 3 and up
npm install -g eslint // Install ESlint, download the latest version
eslint <filename>
```

- You an run eslint globally or project by project
- We are going to create and eslintrc file
```
touch .eslintrc // In the home directory
```
- Set the environment, style, and rules
- Spend time and try to understand the rules
- Rules can be off, warn, or error (0, 1, 2 respectively)
```
{
    "env": {
        "es6": true,
        "browser": true,
        "jquery": true,
    },
    "extends": "eslint:recommended"
    "rules": {
        "no-console": "warn"
    }
}
```

## Airbnb ESLint Settings
- A style guide put out by AirBnB engineers
- Most people adopt AirBnB style guide and then add their own rules
- Install from [ESLint AirBnB](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb)

### 1. Install option 1
- This worked for me
```
npm i -g eslint-config-airbnb-standard
```

### 2. Install option 2
- In config, extend airbnb instead
```
{
    "env": {
        "es6": true,
        "browser": true,
        "jquery": true,
    },
    "extends": "airbnb"
    "rules": {
        "no-console": "warn"
    }
}
```
- Run in terminal to install AirBnB style guide
```
sudo npm install -g eslint eslint-config-airbnb eslint-plugin-jsx-a11y eslint-plugin-import eslint-plugin-react
```
- Remove eslint since we already have it
```
sudo npm install -g eslint-config-airbnb eslint-plugin-jsx-a11y eslint-plugin-import eslint-plugin-react
```
### Working with errors
- We can run "fix" to clear basic spacing errors
- The second part of the error is the error message which we can look on eslint website
- If you don't have any errors, it will not display errors
```
eslint bad-code.js --fix
```
  
- You can have files for local projects and globally
- The .eslintrc is going to be in your home directory
- Wes then went over some bad code and how to add exceptions
- The first one has a warn and parameters also
- You should start using the Airbnb and conform the rules to your needs
```
"no-unused-vars": [1,{ "argsIgnorePattern": "res|next|^err"}],
"comma-dangle": 0,
"no-console": 0,
etc.
```

## Line and File Specific Settings
- Sometimes there are programs that require commands in your file
- Like Google Analytics or Twitter:
```
ga.track();
twttr.trackConversion();
```
### Setting globals in your file
- You should set globals file by file
```
/* globals twttr ga */
```

- Array.prototype.includes() is an ES7 syntax
- You need to polyfill it but usually considered a no-no to change your prototypes
- For example we can disable and enable eslint for the entire file
```
/* eslint-disable no-extend-native */
```
- Disable per line basis
```
/* eslint-disable no-extend native */
/* eslint-enable no-extend-native */
```
- For example you can ignore this entire block of polyfill that we weant
```js
/* eslint-disable no-extend native */
// https://tc39.github.io/ecma262/#sec-array.prototype.includes
if (!Array.prototype.includes) {
    Object.defineProperty(Array.prototype, 'includes', {
        value: function(searchElement, fromIndex) {
            
      // 1. Let O be ? ToObject(this value).
      if (this == null) {
          throw new TypeError('"this" is null or not defined');
      }

      var o = Object(this);

      // 2. Let len be ? ToLength(? Get(O, "length")).
      var len = o.length >>> 0;

      // 3. If len is 0, return false.
      if (len === 0) {
          return false;
      }

      // 4. Let n be ? ToInteger(fromIndex).
      //    (If fromIndex is undefined, this step produces the value 0.)
      var n = fromIndex | 0;

      // 5. If n ≥ 0, then
      //  a. Let k be n.
      // 6. Else n < 0,
      //  a. Let k be len + n.
      //  b. If k < 0, let k be 0.
      var k = Math.max(n >= 0 ? n : len - Math.abs(n), 0);

      function sameValueZero(x, y) {
          return x === y || (typeof x === 'number' && typeof y === 'number' && isNaN(x) && isNaN(y));
      }

      // 7. Repeat, while k < len
      while (k < len) {
          // a. Let elementK be the result of ? Get(O, ! ToString(k)).
        // b. If SameValueZero(searchElement, elementK) is true, return true.
        // c. Increase k by 1. 
        if (sameValueZero(o[k], searchElement)) {
            return true;
        }
        k++;
      }

      // 8. Return false
      return false;
    }
  });
}
/* eslint-enable no-extend-native */
```

## ESLint Plugins
- There are good plugins for eslint
- Visit [Awesome-ESlint](https://github.com/dustinspecker/awesome-eslint#plugins)
- Wes uses it to fix javascript in his html and markdown files
```
"plugins": ["html", "markdown"]
```
- Tip: you can use the glob pattern (ie *.md)

## ESLint inside Atom and Sublime Text
- Ideally, you would want eslint inside your editor
- Every editor should have its own lint integration

### With Sublime
- SublimeLinter 3 is one for Sublime Text
- You need to have package control installed
- Wes has `SublimeLinter` and `SublimeLiner-contrib-eslint`
- You need to have ESLint globally installed
- Run `eslint --version` to check
- Make sure to do a full shutdown on your editor
- You can also set the lint mode
- Wes has it on load/save his editor
- Errors pop on the bottom bar of the screen

### With Atom
- It tells you the errors on the screen
- It brings you to the error docs on click
- You need to install linter and linter-eslint packages

## Only Allow ESLint Passing Code into your git repos
- You can install a linter right in your githug repo
- Can also install a "git-hook", requiring code pass by ESLint rules before merging
- It keeps code quality for everyone consistent
```
git init es6git
```
- There's a hook folder in the .git folder
- Code that runs before something opens

# Module #13 JavaScript Modules and Using npm
## JavaScript Modules and WebPack 2 Tooling Setup
- A JS module file that has functions that you can use in other files or share with other devs
- Browser support for ES6 modules is not as great so we need some tooling
- An example of importing modules:
```js
import slug from 'slug';
import { uniq, shuffle } from 'lodash'; 
import Flickity from 'flickity';
```

### Setting up our modules with NPM
- You can use JSpm or Bowser, but NPM is most popular
- Package JSON will save all the modules for our app
- Try to install other libraries you would use
- Note: JSONP does not work with fetch api but we an install jsonp package
```js
touch app.js // Creating an entry point
npm init // Make a JSON file for the package, makes a package.json
npm install slug --save // Installs the slug module
npm install lodash flickity --save // Install lodash and slug
install jquery --save
install insane --save
install jsonp --save
```
- Then we need to add import statements also in our HTML file
```js
import { uniq } from 'lodash';
import insane from 'insane';
```
- However, you will error when opening the file in the browser
- Reason is that it doesn't know how to handle the import statement
- So now we need another tool: Webpack
- If you ever delete your node module folder since it's large file size
- You can reinstall it
- You don't need the node_modules folder unless you need offline access
- Don't worry too much about what's in those folders
```
trash node_modules
npm install
```

### Setting up webpack and babel
```
npm install webpack --save-dev
npm install babel-loader babel-core babel-preset-es2015-native-modules --save-dev
```
- We also have to enable it
- Wes opened a file called webpack.config.js and provided this code
- He explained all these settings in the video
```js

const webpack = require('webpack');
const nodeEnv = process.env.NODE_ENV || 'production';

module.exports = {
  devtool: 'source-map',
  entry: {
    filename: './app.js'
  },
  output: {
    filename: '_build/bundle.js'
  },
  module: {
    loaders: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        query: {
          presets: ['es2015-native-modules']
        }
      }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false
      },
      output: {
        comments: false
      },
      sourceMap: true
    }),
    new webpack.DefinePlugin({
      'process.env': { NODE_ENV: JSON.stringify(nodeEnv) }
    })
  ]
};
```
## Creating your own Modules
- Wes made an src folder keep all the modules in
- He's going to create 1 function and 2 strings
- He made a config.js for api keys and keys
- You don't need to put the extension on the end in the import statement
```js
// app.js
import apiKey from './src/config';

// config.js
const apiKey = 'abc123';
```
- Variables are scoped to the module and not global
- Check out the [MDN Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) about exporting

- 2 type of exports:
### 1. Default export
- Export as default, import as any name you would like
- You can only have **one default export** per module
- But you can have as many named exports as you want
```js
// app.js
import apiKey from './src/config'; // Can import as the same name
import wesIsCool from './src/config'; // Or can make a custom name, it will automatically understand since there is only 1 default export per module

console.log(apiKey);
console.log(wesIsCool);

// config.js
const apiKey = 'abc123';

export default apiKey;
```

### 2. Named export
- Export as that variable name, import as that same exact name
- Must use brackets for named exports
- Looks like destructuring but not exactly
```js
// app.js 
import { apiKey, url, sayHi, age, dog } from '.src/config';
sayHi('wes');

// config.js
export const apiKey = 'abc123';
export const url = 'http://wesbos.com';

// You can export functions also
export function sayHi(name) {
    console.log(`Hello there ${name}`);
} 
// Can export multiple variables at once
const age = 100;
const dog = 'snickers';
export { age, dog }
```
- You can also rename the variables if you desire using "as"
- It can be done via import or export side
```js
// app.js
import { apiKey as key, old} from './src/config';
console.log(apiKey);
console.log(old);

// config.js 
export const apiKey = 'abc123';

const age = 100;
const dog = 'snickers';
export { age as old, dog }
```

## More ES6 Module Practice
- We are going to make a user and generate a gravatar url string
- Wes also had to download base-64 plugin as well `install base-64 --save`
- Note that base-64 was exported as a default export
- We are also using the slug plugin here
```js
// app.js
import User, { createURL, gravatar} from './src/user';

const wes = new User('Wes Bos', 'wesbos@gmail.com', 'wesbos.com');
const profile = createURL(wes.name);
const image = gravatar(wes.email);

console.log(wes);
console.log(profile);

// user.js
import slug from 'slug';
import { url } from './config';
import base64 from 'base-64';

export default function User(name, email, website) {
    return { name, email, website }
    }
}

export function createURL(name) {
    return `${url}/users/${slug(name)}`;
}

export function gravatar(email) {
    const has = base64.encode(email);
    const photoURL = `https://www.gravatar.com/avatar/${hash}`;
    return photoURL;
}

// config.js
export const url = 'http://wesbos.com';
```

# Module #14 ES6 Tooling
## Tool-Free Modules with SystemJS (+bonus BrowserSync setup)
- SystemJS, Browserify, Rollup also bundle ES6 module
- Doesn't matter which you use and Wes recommends Webpack
- JSPM sits on top of NPM, not an alternative

### SystemJS
- SystemJS runs in the browser with a script in html
- Don't need any of the overheader for bundlers, compilers, and compilers
- It's great for testing but you would not want to use in production
- Need to run the HTML through a server
```js
<script src="https://jspm.io/sysem@0.19.js">
```

### Install browser-sync
- Helps update HTML preview live
- Install with: 
```
sudo npm install browser-sync --save-dev
```
- Add script to package.json: 
```
"server": "browser-sync start --directory --server --files '*.js, *.html, *.css'"
```
- Add script to index.html:
```
<script>
    System.config({ transpiler: 'babel'});
    System.import('./main.js');
</script>
```

### Installing lodash with SystemJS
- SystemJS can install from NPM without having to run commands
- Add import statement in main.js:
```
import { sum, kebabCase } from 'npm:lodash';
```

- Tax example:
```js
// main.js
import { addTax} from './checkout';

console.log(addTax(100, 0.15));

// checkout.js
export function addTax(amount, taxRate) {
    return amount + (amount * taxRate);
}
```

## All About Babel + npm scripts
- ES6 use is possible to use today with support of Babel and polyfilling
- Babel is a JavaScript compiler
- Converts ES6 to ES5 and also experimental features
- You can try to [Babel REPL](https://babeljs.io/repl/) to try it out
- You only need to use Webpack, Browserify with Babel if you're using modules
- Otherwise you won't need it.
1. Make a babel directory
2. Do a git init
3. Install babel with npm
- Use next if you want to do the experimental version
```
npm install babel-cli@next
```
4. Add script to package.json
```
"scripts": {
    "babel": "babel app.js --watch --out-file app-compiled.js"
  }
```

### No further need for a Babel Preset, introduce babel-preset-env
- Presets are a bundle of plugins
- Preset for ES6 was necessary but browsers now are being updated
- Perhaps you only want to update the certain features
- You can use [babel-preset-env](https://github.com/babel/babel/tree/master/experimental/babel-preset-env)
- After selecting the browser, it will tell you what you need to compile down to ES5
1. Install with
```
npm install babel-preset-env@next
```
2. Put settings in package.json instead of a .babelrc file
- This is the same as putting in babelrc
- SIDENOTE: there's a babel-preset-react package as well
```
"babel" : {
  "presets": [
    ["env", {
      "targets": {
        "browsers": ["last 2 versions", "safari >= 7"]
      }
    }]
  ]
}
```
3. Run babel with:
```
npm run babel
```

### Experimental plugins
- Object-rest-spread was an experimental feature
```js
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
```
- So we need to install a plugin
1. Install via npm:
```
npm install --save-dev babel-plugin-transform-object-rest-spread
```
2. Add to babelrc or package.json
- Note that it's not a preset, so put it outside of that array
```
"plugins": ["transform-object-rest-spread"]
```

## Polyfilling ES6 for Older Browsers
- Babel works only syntax but we still need polyfilling
- Methods like Array.from() for ES6, Babel assumes all browsers have this method (not true)
- We have to polyfill to create this method with vanilla javascript
- There are polyfill sections on MDN
- Two major polyfills:
1. Babel Polyfill
- Only if you need modules or polyfilling a lot
- Uses corejs
```
import "babel-polyfill";
```

2. Polyfill.io
- Visit [site](https://polyfill.io/v2/docs/)
- More lightweight and detects user's browser
- It dynamically generates a JS file for the user
- You can also tailor your response with options
- Install with:
```
<script src="https://cdn.polyfill.io/v2/polyfill.min.js></script>
```

# Module #15 Classes
## Prototypal Inheritance Review
- A constructor function always has capital letter
- Prototypal inheritance is when you put a method on the original constructor function and it will be available for all subsequent objects that you make
- See how the bark and cuddle methods are available to both instances of snickers and sunny
- This is because it is added onto the constructor function and not with each instance
- Think of it like adding something on an original blueprint
```js
function Dog(name, breed) {
    this.name = name;
    this.breed = breed;
}

Dog.prototype.bark = function() {
    console.log(`Bark bark! My name is ${this.name}`)
}

const snickers = new Dog('Snickers', 'King Charles');
const sunny = new Dog('Sunny', 'Golden Doodle');

Dog.prototype.bark = function() {
    console.log(`Bark bark! My name is ${this.name} and I'm ${this.breed}`);
}

Dog.prototype.cuddle = function() {
    console.log(`I love you owner!`);
}
```

## Say Hello to Classes
- We will convert the previous constructor function to ES6 classes now
- Our old syntax:
```js
function Dog(name, breed) {
    this.name = name;
    this.breed = breed;
}

Dog.prototype.bark = function() {
    console.log(`Bark bark! My name is ${this.name}`)
}

const snickers = new Dog('Snickers', 'King Charles');
const sunny = new Dog('Sunny', 'Golden Doodle');

Dog.prototype.bark = function() {
    console.log(`Bark bark! My name is ${this.name} and I'm ${this.breed}`);
}

Dog.prototype.cuddle = function() {
    console.log(`I love you owner!`);
}
```
- However there are 2 types of classes: class declaration and class expression
```js
// Class declaration
class Dog {
    
    }

// Class expression
const Dog = class {

    }
```
- To new ES6 class:
- Remember that all the methods inside of a class only need the shorthand
- It's like the methods that we put inside of an object
- Notice we do not have have a comma after the constructor method
```
constructor() versus constructor: function() {}
```
```js
class Dog {
    constructor(name, breed) {
        this.name = name;
        this.breed = breed;
    }
    bark() {
        console.log(`Bark bark! My name is ${this.name}`)
    }
    cuddle() {
        console.log(`I love you owner!`);
    }
}

const snickers = new Dog('Snickers', 'King Charles');
const sunny = new Dog('Sunny', 'Golden Doodle');

snickers;
sunny;
snickers.bark();
sunny.bark();
```

### Static methods
- Remember when we used Array.from() or Array.of()
- It was not a method avaiable for all arrays, only on Array directly
- We can add the same idea to our code with keyword "static"
- You can only call on the constructor array but not on the objects
```js
class Dog {
    constructor(name, breed) {
        this.name = name;
        this.breed = breed;
    }
    bark() {
        console.log(`Bark bark! My name is ${this.name}`)
    }
    cuddle() {
        console.log(`I love you owner!`);
    }
    static info() {
        console.log('A dog is better than a cat by 10 times'); 
    }
}

const snickers = new Dog('Snickers', 'King Charles');
const sunny = new Dog('Sunny', 'Golden Doodle');

sunny.info(); // Will not work
Dog.info(); // Will work
```

### Getters
```js
class Dog {
    constructor(name, breed) {
        this.name = name;
        this.breed = breed;
    }
    bark() {
        console.log(`Bark bark! My name is ${this.name}`)
    }
    cuddle() {
        console.log(`I love you owner!`);
    }
    static info() {
        console.log('A dog is better than a cat by 10 times'); 
    }
    get description() {
        return `${this.name} is a ${this.breed} type of dog`;
    })
}

snickers.description // Will work
```

### Setters and Getters
- A setter is basically setting up how a value would be returned
- A getter is the value that will return when you call that setter
- Read this [article](https://javascriptplayground.com/blog/2013/12/es5-getters-setters/)
```js
class Dog {
    constructor(name, breed) {
        this.name = name;
        this.breed = breed;
    }
    bark() {
        console.log(`Bark bark! My name is ${this.name}`)
    }
    cuddle() {
        console.log(`I love you owner!`);
    }
    static info() {
        console.log('A dog is better than a cat by 10 times'); 
    }
    get description() {
        return `${this.name} is a ${this.breed} type of dog`;
    })
    set nicknames(value) {
        this.nick = value.trim();
    }
}

snickers.nicknames = ` snicky `; // Will work, " snicky "
snickers.nicknames; // does not work, needs a getter

// Add a getter
get nicknames() {
    return this.nick;
}
```

## Extending Classes and using super()
- You can also extend a class 
- Take the animal class and we add on rhino and dog
- When we add dog, he also barks
```js
class Animal {
    constructor(name) {
        this.name = name;
        this.thirst = 100;
        this.tbelly = [];
    }
    drink() {
        this.thirst -= 10;
        return this.thirst;
    }
    eat(food) {
        this.belly.push(food);
        return this.belly;
    }
}

const rhino = new Animal("Rhiney");

rhino;
rhino.eat('burger');
rhino.eat('leaves');
rhino.eat('zebra');
rhino.drink();
```
- Now we extend Animal to Dog
- But you will get an error because Dog is just extending Animal
```js
const snickers = new Dog ('Snickers', 'King Charles');
class Dog extends Animal {
    constructor(name, breed) {
        super()
        this.name = name;
        this.breed = breed;
    }
}
```
- You need to make an Animal before you make the Dog 
- We use super() for that
- It calls on the original item that we are extending
- We do not need to set the name since it's already on the original constructor
- But we do have to add the breed
```js
const snickers = new Dog ('Snickers', 'King Charles');
class Dog extends Animal {
    constructor(name, breed) {
        super(name)
        this.breed = breed;
    }
}
```
- Now we also add bark() on the class:
```js
const snickers = new Dog ('Snickers', 'King Charles');
class Dog extends Animal {
    constructor(name, breed) {
        super()
        this.name = name;
        this.breed = breed;
    }
    bark() {
        console.log('Bark I\'m a dog!');
    }
}
```
- A good rule is not to extend beyond 2 or 3

## Extending Arrays with Classes for Custom Collections
- How do we extend array methods to our new classes?
- We first extend it and then call super() to construct the object
```js
class MovieCollection extends Array {
    constructor(name, ...items) {
        super(...items);
        this.name = name;
    }
}

const movies = new MovieCollection('Wes\'s Fav Movies',
{ name: 'Bee Movie', stars: 10 },
{ name: 'Star Wars Trek', stars: 1 },
{ name: 'Virgin Suicides', stars: 7 },
{ name: 'King of the Road', stars: 8 }
);
```
- We also add methods:
```js
class MovieCollection extends Array {
    constructor(name, ...items) {
        super(...items);
        this.name = name;
    }
    add(movie) {
        this.push(movie);
    }
    topRate(limit = 10) {
        return this.sort((a,b) => (a.stars > b.stars ? -1 :1).slice(0, limit); 
    }
}

const movies = new MovieCollection('Wes\'s Fav Movies',
{ name: 'Bee Movie', stars: 10 },
{ name: 'Star Wars Trek', stars: 1 },
{ name: 'Virgin Suicides', stars: 7 },
{ name: 'King of the Road', stars: 8 }
);

movies.add({ name: 'Titanic', stars: 5 })
console.table(movies.topRated());
console.table(movies.topRaded(2));
```

# Generators
## Introducing Generators
- Generators allows you to pause/stop a function 
- You use `*` after the function keyword and `yield` IE `function* sayHi()`
- You can also put the `*` in front of the function name IE `function *sayHi()`
- `yield` is like a return for now
- It will return that one item until it is called again
- Use .next() to advance through generator
```js
function* listPeople() {
    yield 'Wes';
    yield 'Kait';
    yield 'Snickers';
}

const people = listPeople();

people; // Returns the generator
people.next(); // Object {value: "Wes", done: false}
people.next(); // Object {value: "Kait", done: false}
people.next(); // Object {value: "Snicker", done: false}
people.next(); // Object {value: undefined, done: true}
```
- You can also use with dynamic variables
```js
function* listNumber() {
    let i = 0;
    yield i;
    i++;
    yield i;
    i++;
    yield i;
}

const number = listNumber();

number.next(); // Object {value: 0, done: false}
number.next(); // Object {value: 1, done: false}
number.next(); // Object {value: 2, done: false}
number.next(); // Object {value: undefined, done: true}
```

### Iterating through an array with a generator
- Inventor example:
- We are looping through an array with a generator
- The values will return under value when you call .next()
- TIP: you can tack on the .value() if you don't care about the done value
```js
const inventors = [
{ first: 'Albert', last: 'Einstein', year: 1879 },
{ first: 'Isaac', last: 'Newton', year: 1643 },
{ first: 'Galileo', last: 'Galilei', year: 1564 },
{ first: 'Marie', last: 'Curie', year: 1867 },
{ first: 'Johannes', last: 'Kepler', year: 1571 },
{ first: 'Nicolaus', last: 'Copernicus', year: 1473 },
{ first: 'Max', last: 'Planck', year: 1858 },
];

function* loop(arr) {
    console.log(inventors);
    for (const item of arr) {
        yield item;
    }
})

const inventoryGen = loop(inventors);

// You have to call .next() for it to show in console
inventorGen.next(); // Returns array of objects, Object {value: Object, done: false}
```

## Using Generators for Ajax Flow Control
- Generators can do waterfall-like AJAX requests
- Waterfalls create callback hell and hard to deal with
- Browsers are getting better with dealing with this
- Assume these calls rely on each other and needs waterfall
- After the call is complete, the data is stored back into that variable
```js
function ajax(url) {
    fetch(url).then(data => data.json()).then(data => dataGen.next(data))
}
function* steps() {
console.log('fetching beers');
const beers = yield ajax('http://api.react.beer/v2/search?q=hops&type=beer');
console.log(beers);

console.log('fetching wes');
const wes = yield ajax('https://api.github.com/users/');
console.log(wes);

console.log('fetching fat joe');
const fatJoe = yield ajax('https://api.discogs.com/artists/51988');
console.log(fatJoe);
}

const dataGen = steps();
dataGen.next(); // To initialize generator
```

## Looping Generators with for of
- Now we want to loop through a generator all at once
- There are no pauses and you don't have to keep running .next()
```js
function* lyrics() {
    yield `But don't tell my heart`;
    yield `My achy breaky heart`;
    yield `I just don't think he'd understand`;
    yield `And if you tell my heart`;
    yield `My achy breaky heart`;
    yield `He might blow up and kill this man`;
}

const achy = lyrics();

for (const line of achy) {
    console.log(line);
}
```

# Module #17 Proxies
## What are Proxies?
- Proxies allows modification the default operations of objects
- You can override properties like set, get
- We use this with `new Proxy(target, handlerObject)`
- We set "traps" in a handler to handle the data being passed in
- Check [MDN Handlers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler) for more traps
```js
const person = { name: 'Wes', age: 100 };
const personProxy = new Proxy(person, {
    get(target, name) {
        console.log('Someone is asking for ', target, name);
        return 'Nahhhh';
        // or you can return:
        return target[name].toUpperCase();
    }
})

personProxy.name = 'Wesley';
personProxy; // Proxy {name: "Wesley", age: 100}
personProxy.name; // Someone is asking for Object {name: "Wesley", age: 100} name
personProxy.name; // With the added return, it will return "Nahhhh" now
```
- Now we are going to use set:
- Trim() basically takes away the spaces
- If you do not set a parameter for a trap, the defaults will apply
```js
const person = { name: 'Wes', age: 100 };
const personProxy = new Proxy(person, {
    get(target, name) {
        console.log('Someone is asking for ', target, name);
        return 'Nahhhh';
        // or you can return:
        return target[name].toUpperCase();
    },
    set(target, name, value) {
        if(typeof value === 'string') {
            target[name] = value.trim().toUpperCase();
        }
    }
})

personProxy.cool = "   ohhh yeah   ";
personProxy; // Proxy {name: "Wesley", age: 100, cool: "ohhh yeah"}
personProxy.wes = "I love wes   ";
personProxy.wes = "I LOVE WES";
```

## Another Proxy Example
- Phone numbers are difficult to deal with
- These would be US numbers
```js
const phoneHandler = {
    set(target, name, value) {
        target[name] = value.match(/[0-9]/g).join('');
    },
    get(target, name) {
        return target[name].replace(/(\d{3})(\d{3})(\d{4})/, '($1)-$2-$3');
    }
}

const phoneNumbers = new Proxy({}, phoneHandler);

// Before set
phoneNumbers.home = '+234 -234-2343'; // "+234 -234-2343"
phoneNumbers.work = '234 234 2343'; // "234 234 2343"
phoneNumbers; // Proxy {home: "+234 -234-2343", work: "234 234 2343"}

// After set
phoneNumbers.work = '234 234 2343'; // "234 234 2343"
phoneNumbers.work; // "2342342343"
phoneNumbers.home = '+234 -234-2343'; // "+234 -234-2343"
phoneNumbers.home; // "2342342343"

// After get
phoneNumbers.work = '234 234 2343'; // "234 234 2343"
phoneNumbers.work; // "(234)-234-2343"
phoneNumbers.cell = '1231231234'; 
phoneNumbers.cell; // "(123)-123-1234"
```

## Using Proxies to combat silly errors
- When you are building libraries, you want to make it easy for other developers
- Here are some common casses with map coordinates or people properties:
```js
const map = {};
map.logitiude = 79.3423; // spell wrong
map.longitude = 79.3423; // full spelling
map.long = 79.3423; // wrong key
map.lon = 79.3423; // nope
map.lng = 79.3423; // Got it!

const person = { name: 'Wes' };
person.ID = 123; // no
person.iD = 123; // no
person.id = 123; // yes
```
- So now we will construct a safety proxy:
```js
const safeHandler = {
    set(target, name, value) {
        const likeKey = Object.keys(target).find(k => k.toLowerCase() === name.toLowerCase());

        if(!(name in target)) && likeKey) {
            throw new Error(`Oops! Looks like like we already have a(n) ${name} property but with the case of ${likeKey}.`)
        }
        target[name] = value;
    }
}

const safety = new Proxy({ id: 100}, safeHandler);

safetyID = 200; // Does not work
safety.name = 'wes'; // Will work
safety.Name = 'wesley'; // Does not work
```

# Module #18 Sets and WeakSets
## Sets
- A set is like a unique array that you can only add an item once
- It has useful methods to manage the set also
- Use `new Set()` to initate
- Use .add() to add to the set
- Use .delete() to delete the set
. Use .clear() to clear the set
- Use .size() to see the amount of people
```js
const people = new Set();
people.add('Wes');
people.add('Snickers');
people.add('Kait');

people.size; // 3
people.delete('Wes'); // true, Set(2) {"Snickers", "Kait"}
people.clear(); // undefined
```
- Use .values() or .keys() to see the items
- Use .entries() to see items as key/value pairs
- It is not index-based and can't access items individually
- You can use generator .next() or ForOf loops
```js
people.values(); 

const it = people.values();
it.next();

for (const person of people) {
    console.log(person);
}
```
- Students example:
- Can define the set as you're defining it or from an array variable
- Use .has() to see if it contains an item
- It will not add an item twice
- Can not use index to access it
```js
const student = new set(['Wes', 'Kara', 'Tony']);

const dogs = ['Snickers', 'Sunny'];
const dogSet = new Set(dogs);

dogSet; // Set {"Snickers", "Sunny"}
students; // Set {"Wes", "Kara", "Tony"}
student.has('Tony'); // true
student.has('Wesssss'); // false
students.has('Wes'); // Will not add it twice
students[1]; // Will not work
```

## Understanding Sets with Brunch
- Understanding with a brunch example
- Brunch is like the master list
- Line is a secondary list that is self-managing
- Notice we add Heather and Snickers later to the set
- The iterator will still pass over it
- When .add() to brunch, it automatically gets push to line also (pretty cool!)
```js
const brunch = new Set();

// as people start coming in
brunch.add('Wes');
brunch.add('Sarah');
brunch.add('Simone');

// ready to open!
const line = brunch.values();
console.log(line.next().value); // Wes
console.log(line.next().value); // Sarah
brunch; // Set {"Wes", "Sarah", "Simone"}
line; // SetIterator {"Simone"}

brunch.add('Heather');
brunch.add('Snickers');
console.log(line.next().value); // Simone
console.log(line.next().value); // Heather
console.log(line.next().value); // Snickers
```

## WeakSets 
- Like a set with some limitations
1. Can only contain objects
2. You cannot loop over weaksets
3. There is no .clear() method
    - There's a garbage collection method
    - If code is deleted, it will be removed from memory
- Create with `new WeakSet()`
- Add with `.add()`
```js
let dog1 = { name: 'Snickers', age: 3 };
let dog2 = { name: 'Sunny', age: 1 };

const weakSauce = new WeakSet([dog1, dog2]);
weakSauce.add(dog2);

weakSauce; // WeakSet {Object { name: 'Snickers', age: 3 }, Object { name: 'Sunny', age: 1 }}
weakSauce.has(dog1); // true
weakSauce.has(dog2); // true

// Will not work because we can't enumerate over a weakset
for (const dog of weakSauce) {
    console.log(dog);
}
```

### Garbage Collection with WeakSets
- No way to force the garbage collection process, depends on browsers, etc.
- You might experience a memory leak since it takes a couple seconds to delete variable
- If variables are not declared or defined null, it will be removed
- Helps prevent memory leaks since it self-manages
```js
let dog1 = { name: 'Snickers', age: 3 };
let dog2 = { name: 'Sunny', age: 1 };

const weakSauce = new WeakSet([dog1, dog2]);

// Why does Snickers still appear?
console.log(weakSauce); // WeakSet {Object { name: 'Snickers', age: 3 }, Object { name: 'Sunny', age: 1 }}
dog1 = null;
console.log(weakSauce); // WeakSet {Object { name: 'Snickers', age: 3 }, Object { name: 'Sunny', age: 1 }}

// It takes time to dissapear
// After 3 seconds or so...
console.log(weakSauce); // WeakSet {Object { name: 'Sunny', age: 1 }}
```

# Module #19 Map and WeakMap
## Maps
- Maps works on objects
- Works similar like sets to arrays except it has keys and values
- Initiate with `new Map()`
- Add entries with `.set()`
- Search with `.has()`
- Get values with `.get()`
- Map size with `.size()`
- Delete entries with `.delete()`
- More at [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)
```js
const dogs = new Map();

dogs.set('Snickers', 3);
dogs.set('Sunny', 2);
dogs.set('Hugo', 10);

dogs.has('Snickers'); // true
dogs.get('Snickers'); // 3
dogs.delete('Hugo'); // true
dogs; // Map {"Snickers" => 3, "Sunny" => 2}
```

### Looping over Maps
- Can use forEach or forOf
- With forEach:
```js
dogs.forEach((val, key) => console.log(val, key));

// Returns
// 3 "Snickers"
// 2 "Sunny"
// 10 "Hugo"
```
- With forOf:
- Returns arrays so we can actually deconstruct:
```js
for (const dog of dogs) {
    console.log(dog);
}

// Returns
// ["Snickers", 3];
// ["Sunny", 2];
// ["Hugo", 10];
```
- To deconstruct:
```js
for (const [key, val] of dogs) {
    console.log(key, val);
}

// Returns
// Snickers 3
// Sunny 2
// Hugo 10
```

## Map Metadata with DOM Node Keys
- Sometimes we don't want metadata on our object, just about the object
- So we can create a metadata Map to hold this information
- Like how many times we click each button
- We want to use an object as the key
```js
<button>Snakes 🐍</button>
<button>Cry 😂</button>
<button>Ice Cream 🍦</button>
<button>Flamin' 🔥</button>
<button>Dancer 💃</button>

const clickCounts = new Map();
const buttons = document.querySelectorAll('button');

buttons.forEach(button => {
    clickCounts.set(button, 0);
    button.addEventListener('click', function() {
        const val = clickCounts.get(this);
        clickCounts.set(this, val + 1);
        console.log(clickCounts);
    })
})

clickCount; // Returns a map
```

## WeakMap and Garbage Collection
- WeakMap is similar as WeakSet
- Can't tell the size on it
- You can't loop over it
- Items will get garbage collected if not exists in your file
- WeakMap doesn't have a size
```js
let dog1 = { name: 'Snickers' };
let dog2 = { name: 'Sunny' };

const strong = new Map();
const weak = new WeakMap();

strong.set(dog1, 'Snickers is the best!');
weak.set(dog2, 'Sunny is the 2nd best!');

strong; // Map
weak; // WeakMap
weak.size; // undefined
strong.size; // 1

// Let's delete variables
dog1 = null;
dog2 = null;
strong; // Map
weak; // WeakMap
```

# Module #2 Async + Await Flow Control
## Async Await - Native Promises Review
- Review for promises
- Newer browsers are promise-based and don't have to define success and error anymore
- We just have to define .then() and .catch()
- There are 2 built-in promises API: fetch API and navigator API
- We are using fetch API now
```js
fetch('https://api.github.com/users/wesbos') // Returns a promise
.then(res => return res.json()) // Fetch does not assume JSON
.then(res => console.log(res))
.catch(err => {
    console.error('OH NO!!!');
    console.error(err);
})
```

### Get user media with navigator API: webcam
- We are now working with webcam
- Add a video element `<video controls class="handsome"></video>`
- Then in JS:
```js
const video = document.querySelector('.handsome');

navigator.mediaDevices.getUserMedia({ video: true })
.then(mediaSream => {
    video.srcObject = mediaStream;
    video.load();
    video.play();
})
.catch(err => console.log(err));

// An alternative to video.srcObject
// video.src = window.URL.createObjectURL(mediaStream);
```

## Async Await - Custom Promises Review
- Anything that returns promises has built-in APIs which we can use .then()/.catch() or async/await
- Doing callbacks inside callbacks is inefficient
```js
// Nested callbacks
breathe(1000, function() {
    breathe(2000, function() {
        breathe(4000, function() {

        })
    })
})
```
- Async/await can handle the same problem much better
```js
function breathe(amount) {
    return new Promise((resolve,reject) => {
        setTimeout(() => resolve('Done for ${amount} ms!'), amount);
    })
}

breathe(1000)
.then(res => {
    console.log(res);
    return breathe(500);
}).then(res => {
    console.log(res);
    return breathe(600);
}).then(res => {
    console.log(res);
    return breathe(200);
}).then(res => {
    console.log(res);
    return breathe(500);
}).then(res => {
    console.log(res);
    return breathe(2000);
}).then(res => {
    console.log(res);
    return breathe(250);
}).then(res => {
    console.log(res);
    return breathe(300);
}).then(res => {
    console.log(res);
    return breathe(600);
})
```
- Then we want to reject is ms is less than 500
- We want to catch errors also
```js
function breathe(amount) {
    return new Promise((resolve,reject) => {
        if (amount < 500) {
            reject('That is too small of a value');
        }
        setTimeout(() => resolve('Done for ${amount} ms!'), amount);
    })
}

breathe(1000)
.then(res => {
    console.log(res);
    return breathe(500);
}).then(res => {
    console.log(res);
    return breathe(600);
}).then(res => {
    console.log(res);
    return breathe(200);
}).then(res => {
    console.log(res);
    return breathe(500);
}).then(res => {
    console.log(res);
    return breathe(2000);
}).then(res => {
    console.log(res);
    return breathe(250);
}).then(res => {
    console.log(res);
    return breathe(300);
}).then(res => {
    console.log(res);
    return breathe(600);
}).catch(err => {
    console.error(err);
    console.error('You screwed up!');
})        
```

## All About Async + Await
- In the previous lesson, it was still messy
- Now we will convert it to async/await
- Asynchronous means you start the task and immediately move on to next task
- Things like alert() and prompt() will block tasks
- Console.log() will just interject but doesn't block
```js
// This returns a promise so it is not blocking
const res = fetch('https://github.com');
console.log(res);

// This will block the date from running
alert('hi'); 
setInterval(() => console.log(Date.now()), 500)
```
- Await means we want a response to come back before starting the next function
- Async/await is inherently built in promises, it is not an alternative
- You need to put await inside of an async function, or else it won't work (top-level await)
- To initiate:
```js
// Method 1
const go = async () => {

}

// Method 2
async function go() {

}
```
- Now we use the breathe function again
```js
function breathe(amount) {
    return new Promise((resolve,reject) => {
        if (amount < 500) {
            reject('That is too small of a value');
        }
        setTimeout(() => resolve('Done for ${amount} ms!'), amount);
    })
}

async function go() {
    console.log('start');
    const res = await breathe(1000);
    console.log(res);
    const res2 = await breathe(600);
    console.log(res2);
    const res3 = await breathe(750);
    console.log(res3);
    const res4 = await breathe(900);
    console.log(res4);
    console.log('end');
}

go();
```

## Async + Await Error Handling
- In the previous lesson, if the value is less 300ms, we didn't catch the error
- Now we learn about error handling
- Initiate with a `try {}` and `catch(err) {}` block
- This is method 1:
```js
function breathe(amount) {
    return new Promise((resolve,reject) => {
        if (amount < 500) {
            reject('That is too small of a value');
        }
        setTimeout(() => resolve('Done for ${amount} ms!'), amount);
    })
}

async function go() {
    try {        
        console.log('start');
        const res = await breathe(1000);
        console.log(res);
        const res2 = await breathe(600);
        console.log(res2);
        const res3 = await breathe(750);
        console.log(res3);
        const res4 = await breathe(900);
        console.log(res4);
        console.log('end');
    } catch (err) {
        console.error('Ohhh noo!!!');
        console.error(err);
    }
}

go();
```

### Using high-order function
- This was use in node.js
- Problem was it was difficult for 30 or more pages
- You would have to repeat the same code
- This is an example of where it was discovered:
```js
const userPage = router.get('/user', async (req, res, next) => {
    try {
        const users = await User.find(); // 10 users
    } catch(e) {
        res.render('error')
    }
})
```
- We can use an umbrella function to achieve a solution
- Higher order function takes in another function and returns a modified
```js
function catchErrors(fn) {
    return function() {
        return fn().catch((err) => {
            console.error('Ohh nooo!!');
            console.error(err);
        });
    }
}

async function go() {
        console.log('start');
        const res = await breathe(1000);
        console.log(res);
        const res2 = await breathe(600);
        console.log(res2);
        const res3 = await breathe(750);
        console.log(res3);
        const res4 = await breathe(900);
        console.log(res4);
        console.log('end');
    } 
}

const wrappedFunction = catchErrors(go);

wrappedFunction();
```
- We can also do a spread on argments
```js
function catchErrors(fn) {
    return function(...args) {
        return fn(...args).catch((err) => {
            console.error('Ohh nooo!!');
            console.error(err);
        });
    }
}

async function go(name, last) {
        console.log(`Starting for ${name} ${last}`);
        const res = await breathe(1000);
        console.log(res);
        const res2 = await breathe(600);
        console.log(res2);
        const res3 = await breathe(750);
        console.log(res3);
        const res4 = await breathe(900);
        console.log(res4);
        console.log('end');
    } 
}

const wrappedFunction = catchErrors(go);

wrappedFunction('Wes', 'Bos');
```

## Waiting on Multiple Promises
- Be careful when running multiple promises
- We often want to fire off multiple fetch and wait for results
- We can put await on each variable but it will be coming back at different times
- What if we want to wait until all of them come back?
- We can initiate with a `Promise.all`
- We can get data back by
1. Adding on .then()
```js
async function go() {
    const p1 = fetch('https:/api.github.com/users/wesbos').then(r => r.json();
    const p2 = fetch('https://api.github.com/users/stolinski').then(r => r.json());
    // Wait for both of them to come back
    const res = await Promise.all([p1, p2]);
    
}
```
2. Map over the array of promises
```js
async function go() {
    const p1 = fetch('https:/api.github.com/users/wesbos');
    const p2 = fetch('https://api.github.com/users/stolinski');
    // Wait for both of them to come back
    const res = await Promise.all([p1, p2]);
    const dataPromises = res.map(r => r.json());
    const wesAndScott = await Promise.all(dataPromises);
    console.log(wesAndScott);

    // You can also destructure the array immediately
    const [wes, scott] = await Promise.all(dataPromises);
    console.log(wes, scott);
}
```
- Another way to write the code by passing in an array to the promise
```js
async function getData() {
    const promises = names.map(name => 
        fetch(`https:/api.github.com/users/${name}`)
        .then(r => r.json())
    );
    const [people] = await Promise.all(promises);
    console.log(people);
}

getData(['wesbos', 'stolinski', 'darcyclarke']);
```
- We can also use .race() if we want the first response to come back
- Resolves when the first response comes back
- Not common in real world scenarios
- Initiate with `.race()`
```js
async function getData() {
    const promises = names.map(name => 
        fetch(`https:/api.github.com/users/${name}`)
        .then(r => r.json())
    );
    const [people] = await Promise.race(promises);
    console.log(people);
}

getData(['wesbos', 'stolinski', 'darcyclarke']);
```

## Promisifying Callback Based Functions 
- If you want to promisify a callback
- ES6-Promisify library or Node has a built-in utility for this also
- We are using an older api to get geolocation coordinates
- But now we want to make it a promise
```js
    navigator.geolocation.getCurrentPosition(function(pos) {
        console.log('it worked!');
        consolle.log(pos);
    }, function(err) {
        console.log('it failed!');
        consolle.log(err);
    })
```
- We can promisify with:
```js
function getCurrentPosition(...args) {
    return new Promise((resolve, reject) => {
        navigator.geolocation.getCurrentPosition(...args, resolve, reject);
    })
}

async function go() {
    console.log('starting');
    const pos = await getCurrentPosition();
    console.log(pos);
    console.log('finished');
}

go();
```

# Module #21 ES7, ES8 + Beyond
## Class Properties
- Class appears in ReactJS
- When we were working with the Class Dog, say we want to add bark count property
- It didn't have class properties aka class initializers
- We had to define it in the constructor first
- Often, you'll want a lot of initializers and don't want to tack it in the constructor all the time
```js
class Dog {
    constructor(name, breed) {
        this.name = name;
        this.breed = breed;
        this.barks = 0;
    }
    bark() {
        console.log(`Bark Bark! My name is ${this.name}`)
        this.barks = this.barks + 1;
        console.log(this.barks);
    }
}

const snickers = new Dog('Snickers', 'King Charles');
const sunny = new Dog('Sunny', 'Golden Doodle');
)
```
- Now there's an initializer being proposed to add it
- For now, you can use [Babel transform](https://babeljs.io/repl/)
```js
class Dog {
    constructor(name, breed) {
        this.name = name;
        this.breed = breed;
    }
    barks = 0; // This is the new property

    bark() {
        console.log(`Bark Bark! My name is ${this.name}`)
        this.barks = this.barks + 1;
        console.log(this.barks);
    }
}

const snickers = new Dog('Snickers', 'King Charles');
const sunny = new Dog('Sunny', 'Golden Doodle');
)
```
## padStart and padEnd
- Two new string methods that adds spaces or characters
- The default is spaces
```js
'wes'.padStart(6) // "   wes"
'wes'.padEnd(6) // "wes   "
'wes'.padEnd(6, 'X') // "wesXXX"
'wes'.padEnd(6, '_') // "wes___"
```
- If we want to do it with an array:
```js
const strings = ['short', 'medium size', 'thisis really really long'];

strings.forEach(str => console.log(str.padStart(3)));
```
- If we want to figure out the length
```js
const strings = ['short', 'medium size', 'thisis really really long'];
const longestString = strings.sort(str => str.length).map(str => str.length)[0];
console.log(longestString);

strings.forEach(str => console.log(str.padStart(longestString)));
```
- We can also use it say in podcast numbers
```js
"1".padStart(3); // "  1"
"1".padStart(3, 0); // "001"
"1".padStart(3, "hi"); // "hi1"
"1".padStart(9, "wes"); // "wesweswe1"
```

## ES7 Exponential Operator
```js
['a','b','c'].includes('c'); // true


3 ** 3; // 27
Math.pow(3, 3); // 27
2**2**2; // 16
Math.pow(2,2,2); // 4
Math.pow(Math.pow(2,2),2); // 16
```

## Function Arguments Trailing Comma
- Now you can leave trailing commas
- Problems arose in a team work environment
- You can set up Prettier or ESLint to do this also
```js
const names = ['wes', 'kait', 'lux',];
const names = [
    'wes', 
    'kait', 
    'lux',
    'poppy',
    ];

const people = {
    wes: 'cool',
    kait: 'even cooler!',
    lux: 'coolest',
    poppy: 'Smallest',
    snickerss: 'Bow wow',
}

function family(
    mom,
    dad,
    children,
    dogs,
) {}
```

## Object.entries() and Object.values()
- Now we have 2 new methods on objects: `Object.entries()` and `Object.values()`
- We always had `Object.keys()`
- An example:
```js
const inventory = {
    backpacks: 10,
    jeans: 23,
    hoodies: 4,
    shoes: 11
}

Objects.keys(inventory); // Gives you an array of keys
Objects.values(inventory); // Gives you an array of values
Objects.entries(inventory); // Gives you an array of both keys and values

// Make a nav for the inventory
const nav = Objects.keys(inventory).map(item => `<li>${item}</li>`).join('');
console.log(nav);

// Tell us how many values we have
const totalInventory = Object.values(inventory).reduce((a,b) => a + b);
console.log(totalInventory);

// Print an inventory list with numbers
Object.entries(inventory).forEach(pair => {
    const [key, val] = pair;
    console.log(key, val);
})

// Can also destructure as it comes in
Object.entries(inventory).forEach([key, val] => {
    console.log(key, val);
})
```
- They are all iterables
```js
for (const [key, val] of Object.entries(inventory)) {
    console.log(key);
    console.log(val;
}
```
- Benefit of using forOf instead of forEach is the ability to break out
- Wes prefers to use a .map()
```js
for (const [key, val] of Object.entries(inventory)) {
    console.log(key);
    if (key === 'jeans') break;
}
```