# Prototype Pollution
Tags:
Related to:
See also:
Previous:

## Links
Title: [Olivier Arteau -- Prototype pollution attacks in NodeJS applications](https://www.youtube.com/watch?v=LUsiFV3dsK8 "Olivier Arteau -- Prototype pollution attacks in NodeJS applications")

Title: Your Code can be polluted. Here's how
Link https://www.youtube.com/watch?v=XS_UMqQalLI

## Notes
Everything in JS is an object
Object has a property called `__proto__` that serves as a base class.

Example

```
let someObject = {}
let user = {}

console.assert(user.isAdmin === true) // false
someObject.__proto__.isAdmin = true 
console.assert(user.isAdmin === true) // true now!
```
