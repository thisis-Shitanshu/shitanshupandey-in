---
layout: post
title: "JavaScript"
categories: JavaScript Basics
published: true
---

Notes on JavaScript
- `"use strict";` in first line of file or function to catch common errors in JS.

# #Data Types
- Seven Data Type:
    - undefined
    - null
    - boolean
    - string
    - symbol
    - number
    - object

# #Variables
- Declaring variable:
    - `var myName = "Shitanshu"`
        - Global/Local scope.
    - `let myName = "Shitanshu"`
        - Block scope.
        - Does not let you **declare** a variable twice in same scope.
    - `const PI = 3.14`
        - Read only.

# #String
- Escape characters:
    - A string starting with single quotes, won't need to escape double quotes inside.
        - single quotes: \\'
        - double quotes: \\"
    - backslash: \\
    - newline: \n
    - carriage return: \r
    - tab: \t
    - backspace: \b
    - form feed: \f 

- Finding length:
    - `let stringLength = string.length;`

- Bracket Notation to find characters:
    - `let firstLetter = firstName[n];`

- String Imutabilty:
    - They cannot be altered.
    - Single characters cannot be changed but whole string can be.

# #Array
- Can store multiple datatype in single array.
    - `let myArray = ["John", 34];`

- Append at the end with `push()`:
    - `myArray.push(["dog", 3]);`

- Remove last element with `pop()`:
    - `myArray.pop();`

- Remove first element with `shift()`:
    - `myArray.shift();`

- Add element on the beignning with `unshift()`:
    - `myArray.unshift(["Happy"]);`

- Converting array to string for browsers:
    - `JSON.stringify([1, 2, 3, 4, 5]);`

- Mutate an Array declared with const:
    - `const s = [5, 6, 7];`
        - `s[0] = 1; // No Error`
    
- Spread Operator to Evaluate Arrays In-Place:
    - Looks like the Rest Operartor.
        - But it Expands the contents (Creates a copy that doesn't take on the modification of parent array) of already Existing Array into another.
    - Can only use in an argument to a function or an object to a literal.
    - `arr2 = [...arr1];`

- Destructuring Assignment to assign Variables from Arrays:
    - `const [z, x, , y] = [1, 2, 3, 4];`
    - `[a, b] = [b, a];`

- Destructuring Assignment with the Rest Operator:
    - `const [, , ...arr] = list; // Missing first two elements.`

# #Operator
- Equality and Inequality operator (not Strict Equality and Strict Inequality) performs Type Conversion.
    - `console.log(10 == "10"); //true`
    - `console.log(10 === "10"); //false`

# #Objects
- Similar to arrays but you use properties, instead of indexes to access the values.
    - The values can be of any datatypes.

- Access property:
    - Dot-notation:
        - `let name = obj.name;`
    - Bracket notation:
        - Used mostly when proprties have space between.
        - `let name = obj["full name"];`
    - Variables:

        ```javascript
        let testObj = {
            12: 'Namaen',
            14: 'Jack',
            16: 'Holly'
        }

        let playerNumber = 14;
        let player = testObj[playerNumber];
        ```

- Updating Object properties to new value:
    - Dot-notation:
        - `obj.name = "Shitanshu";`

- Add new Properties to an Object:
    
    ```javascript
    let testObj = {
        12: 'Namaen',
        14: 'Jack',
        16: 'Holly'
    }
    ```
    - Dot-notation:
        - `testObj.18 = 'Camroon';`
    - Bracket notation:
        - `testObj['19'] = 'Cayezer';`

- Delete Properties:
    - `delete testObj.18;`

- Check object for property:
    - `console.log(testObj.hasOwnProperty('17'));`

- Prevent Object Mutation:
    ```javascript
    const MATH_CONSTANTS = {
        PI: 3.14
    };

    Object.freeze(MATH_CONSTANTS);
    ```
- Destructuring Assignment to Assign Variables from Objects:
    - `const { x: { x1: a1 }, y: b, z: c } = voxel;`

- Converting Object to Array:
    - `Object.keys(testObject)`

- **Concise Declarative Function in an Object:**
    ```javascript
    const bicycle = {
        gear: 2,
        setGear(newGear) {
            "use strict";
            this.gear = newGear;
        }
    };

    bicycle.setGear(3);
    console.log(bicycle.gear);
    ```

- Using getter and setters to Control Access to an Object.
    - Variable declared by `_` are usually considered not to be acessed outside the class.

# #Math Functions
- Random:
    - Between 0 and 1: `Math.random();`
    - Whole: 
        - `Math.floor(Math.random() * 20); // 0 and 19`
    - Whole Number within a Range: 
        - `Math.floor(Math.random() * (Max - Min + 1)) + Max; // Gets 0 to Min than adds Max.`

- Use the parseInt with **Radix**:
    - Radix specifies the base of a number in a string.
    - `parseInt("10011", 2);`

# #Function
- GOTCHA: If a varible is defined without a declaration inside a function, it is default to have a Global Scope.

- **Higher Order Arrow Functions:**
    - Examples: map, filter and reduce.
    - Takes functions as arguments for processing collecction of data.

- The **Rest Operator** with Function Parameters:
    ```javascript
    const sum = (function() {
        return function sum(...args) {
            return args.reduce((a, b) => a + b, 0);
        };
    })();
    console.log(sum(1, 2, 3, 4));    
    ```

- Class Syntax to Define a **Constructor Function**:
    - Old:

        ```javascript
        let SpaceShuttle = function(targetPlanet) {
            this.targetPlanet = targetPlanet;
        }
        let zeus = new SpaceShuttle('Jupiter');
        console.log(zeus.targetPlanet);
        ```
    - New:

        ```javascript
        class SpaceShuttle {
            constructor(targetPlanet) {
                this.targetPlanet = targetPlanet;
            }
        }
        let zeus = new SpaceShuttle('Jupiter');
        console.log(zeus.targetPlanet);
        ```

# #Import and exports
- Import everything:
    - `import * as capitalizeString from "dir";`

- Create an export fallback with export deafult:
    - `export deafult function name() { return 4;}`

# #Local Storage and Session Storage
- window object in the browser has both localStorage and sessionStorage.
    - localStorage persists untill we clear it out.
        - `window.localStorage.setItem('myItem', JSON.stringify({ name: 'Shitanshu' }))`
            - Store Strings only in key value pair.
        - `const myRetrievedObject = window.localStorage.getItem('myItem')`
        - `JSON.parse(myRetrievedObject)`
    - sessionStorage persists until the tab currently is not closed.

# #Falsy Value in JavaScript
- 0
- false
- undefined
- null
- NaN
- ""

# #Curry Functions
- Closures are used to build curry functions.

```js
let add = function(a) {
    return function(b) {
        return a + b;
    }
};

let addToFive = add(5);

addToFive(1); //6
```

# #Data Normalization
- Concept of storing lists of elements inside of an object instead of an array is called Data Normalization.

# #Generator Function
- Native to JS, function that resembles async-await.
    - Async-Await is built on top of generator function.
- They pause the execution of the function when they see a specific key, "yield".

```js
function* gen() {
    console.log('a');
    console.log('b');
}

const g = gen();    // Return generator object.
g.next();           // Execution. Return value: undefined, done: true.
```
- Using Yield:

```js
function* gen(i) {
    yield i;
    yield i + 10;
    return 25
}

const g = gen(5);
const gObj = g.next();
gObj // { value: 5, done: false }
const jObj = g.next();
jObj // { value: 15, done: false }
g.next() // { value: 25, done: true }
```

# #Set Collection:
- It is collection object, similar to an array but only adds an item to the list if its unique.
    - If we add the same function, object and the reference already exists than it won't add it.

```js
const function = new Set();
function.add(name);
```