# My Quick Reference: ES6 For Everyone
## By Wes Bos

# Module #1 New Variables - Creation, Updating, Scoping
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

# Module #2 Function Improvements: Arrow and Default Arguments
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

# Module #3 Template Strings
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

# Module #4 Additional String Improvements
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

# Module #5 Destructuring
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

# Module #6 Iterables & Looping
- You can't use break or continue with `.forEach()`
24. `for in` Loop
    - Works on all iterables
    - Returns the index
    - Will also return all prototype methods that were added
    - Recommended for use with objects at the current time
```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

for (const index in cuts) {
    console.log(cuts[index]);
}
```

25. `for of` Loop
    - Works on all iterables EXCEPT objects
    - Currently, `Object.entries()` for objects is being implemented
    - Meantime, you can use `Object.keys()` or the `for in` loop
    - Able to break and continue the loop
    - Can use with `arguments` keyword
```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

for (const index of cuts) {
    if(cut === 'Brisket') {
        break;
    }
    console.log(cut);
}
```

26. `.entries()`
    - Gives you the ArrayIterator and gives you index and value
    - You can advance with `.next()`
    - I think this is a generator
```js
const meat = cuts.entries(); // ArrayIterator

meat.next(); // Returns the next object in the array
```

27. `arguments` keyword
    - Returns an array-ish because it has the Symbol.iterator
    - Normally, we just convert to an array and use `.reduce()`
```js
function addUpNumbers() {
    console.log(arguments); // Returns an arrayish, not an exact array
}

addUpNumbers(10,23,52,34,12,13,123);
```

28. `Object.keys`
    - Returns an array of all the keys in an object
    - Current substitute for `for of` with objects

# Module #7 An Array of Array Improvements    
29. `Array.from()`
    - Not on the prototype, on the Array itself
    - Converts something that is array-ish to a true array
    - 2nd argument takes function
    - Can covert `arguments` (not a true array) into an array also
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

30. `Array.of()`
    - Not on the prototype, on the Array itself
    - Converts everything in your arguments to a true array
```js
const ages = Array.of(12,4,23,62,34);
console.log(ages); // returns an array of these numbers
```

31. `Array.find()`
    - Returns true or false if values exist
    - Use `.filter()` for multiple values
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

32. `Array.findIndex()`
    - Returns where the values exists
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

33. `.some()`
    - Returns if at least one value is true
```js
const ages = [32, 15, 19, 12];
const youngins = [1, 2, 2, 5];

// Is there at least one adult in the group? (>18 years old)
const adultPresent = ages.some(age => age >= 18);
console.log(adultPresent); // true
console.log(youngins); // false
```

34. `.every()`
    - Returns if all values are true
```js
const ages = [32, 15, 19, 12];
const youngins = [1, 2, 2, 5];

// Is everyone old enough to drink? (>19 years old)
const allOldEnough = ages.every(age => age >= 19);
console.log(allOldEnough); // false
```

# Module #8 Say Hello to ...Spread and ...Rest
35. Spread Parameter
- Usage is `...` before the variable
- Takes items in an iterable (ie array) and spreads it out
- If it's a string, it spreads each letter
- Spread is useful for copying arrays also
```js
// Standard syntax
const featured = ['Deep Dish', 'Pepperoni', 'Hawaiian'];
const specialty = ['Meatzza', 'Spicy Mama', 'Margherita'];

// Spreading
const pizzas = [...featured, 'veg', ...specialty];
    // ['Deep Dish', 'Pepperoni', 'Hawaiian', 'veg', 'Meatzza', 'Spicy Mama', 'Margherita'];

// Copying
const fridayPizzas = [...pizzas];
```

36. Rest Parameter
    - Usage is `...` before ther variable
    - Takes items and packs it together in an array

# Module #9 Object Literal Upgrades
37. Same name key-values of objects
    - Can just use key name since values has the same name
    - Can rename the key if you desire
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

38. Working with methods of a function
    - Can short-hand the method for clarity
```js
// Standard syntax
const modal = {
    create: function() {

    },
    open: function() {

    }
}

// ES6 syntax
// You should not use arrow method with methods of a object
// You can add arguments as normal
const modal = {
    create() {

    },
    open(content) {

    }
```

39. Computed Property Names
- Basically allows you to tack on a name for key names
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

console.log(tShirt); // Object{pocketColor: "#ffc600", pocketColorOpposite: "#0039ff"}

// In the past, you had to make a tShirt object and update that object
const tShirt = {};

tShirt[key]: value,
tShirt[`${key}Opposite`]: invertColor(value)
```

40. `.shift()`
- Helps attach values of two arrays at same indices
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