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
7. `.filter()`
    - Create a new array
8. Default arguments