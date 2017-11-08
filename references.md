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

# Module #4
12. `.startsWith()`
    - [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/startsWith)
13. `.endsWith()`
    - [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith)
14. `.includes()`
    - [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)
15. `.repeat()`
    - [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)