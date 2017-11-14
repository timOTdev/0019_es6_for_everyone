# My Quick Reference: ES6 For Everyone
## By Wes Bos

# Module #1
1. `var` keyword 
    - Function scoped

2. `let` keyword 
    - Block scoped
    - Made to be updated
    - Declared once per block
    - Cannot access variable before it is defined

3. `const` keyword 
    - Block scoped
    - Made to be static
    - Declared once per file
    - Can mutate properties of the object
    - Cannot access variable before it is defined

4. `Object.freeze()`
    - Locks properties of an object

# Module #2
5. Arrow function
    - More concise
    - Has implicit returns
    - Doesn't rebind the value of `this`, it inherits the value from the parent
    - Are nameless functions but can be assigned to a variable
    - Use parentheses around an object to get an object literal
    - You don't have access to `arguments` keyword with arrow funtions

6. `.map()`
    - Creates a new array
    - [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

7. `.filter()`
    - Create a new array
    - [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

8. Default arguments
    - You can set a default value for a parameter

# Module #3
9. Template Strings
    - Use backticks and dollar sign + curly brackets
    - Can make multi-line strings
    - Can nest template strings inside another template string
    - Can use features like .map(), ternary operators
    - Can pass functions in a template tags

10. Tagged template literals
    - Put function name in front of template string
    - Can build the string yourself essentially

11. `.reduce()`
    - [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce?v=a)

12. `<abbr>`
    - Creates a mini pop up if the abbreviation has a full definition that is set

13. [DOM Purify](https://github.com/cure53/DOMPurify)
    - Library to clean up malicious code
    - Add a script to the bottom of the body `<script type="text/javascript" src="dist/purify.min.js"></script>`
    - The code to run it is `var clean = DOMPurify.sanitize(dirty);`

# Module #4
14. `.startsWith()`
    - [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/startsWith)
    - 1st paramenter is what you're searching for
    - 2nd parameter is a number of the index where to start searching

15. `.endsWith()`
    - [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith)
    - 1st paramenter is what you're searching for
    - 2nd parameter is a number where only values before this index is considered

16. `.includes()`
    - [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)
    - 1st paramenter is what you're searching for
    - 2nd parameter is a number of the index where to start searching

17. `.repeat()`
    - [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)
    - 1st parameter takes in how many times to repeat the string

# Module #5
18. Destructuring Objects
    - Done with `const { <key> } = <object>`
    - Expand `<object>` for deeper nested keys
    - Use `{ <key>:<rename> }` to rename to another alias

19. Default values with destructuring
    - Provides default values if not given
    - Use `=` to set defaults
```js
var settings = {width: 300, color: 'black'}
const { width = 100, height = 100, color = 'blue', fontSize = 25 } = settings;
```
- Renames with setting defaults also possible
```js
const { w: width = 400, h: height = 500 } = settings;
```

20. Destructuring Arrays
    - Use `[ ]` to destructure arrays
    - If no name given in destructure, other indices are not returned
```js
const team = ["Wes", "Harry", "Sarah", "Keegan", "Riker"];
const [captain, assistant, players] = team;
```

21. Switching variables
    - Use `[] = []` to switch variables
```js
// Standard syntax
let inRing = 'Hulk Hogan';
let onSide = 'The Rock';

console.log(inRing, onSide); // Hulk Hogan The Rock
[inRing, onSide] = [onSide, inRing];
console.log(inRing, onSide); // The Rock Hulk Hogan
```

22. Destructuring objects in functions
- The order of the destructuring does not matter
- Can also destruture only some of the keys also
```js
function convertCurrency(amount) {
    const converted = {
        USD: amount * 0.76,
        GPD: amount * 0.53,
        AUD: amount * 1.01,
        MEX: amount * 13.30
    };
}

// The order does not matter since it's an object
const { MEX, USD, AUD, GPB } = convertCurrency(100);
const { USD, AUD } = convertCurrency(100);
console.log(USD, GPB, AUD, MEX ); // {76, 53, 101, 1330} 
console.log(USD, AUD); // Can choose and pick which to return
```

23. Destructure objects as they are passed as arguments
- Can also destruture objects as they enter functions
- We destructure by using `{ }`
- You can put them in any order and leave off things as well
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

- If we want to destructure a group, we can use `( {parameter, parameter} = {} )`
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