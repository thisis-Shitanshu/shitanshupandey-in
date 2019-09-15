---
layout: post
title: "TypeScript: Part II"
categories: TypeScript VariableDeclaration
published: true
---

A cheatsheet on TypeScript.
Find additional TypeScript configuration Gists, [here.](https://gist.github.com/thisis-Shitanshu/4159f38d651861540ba52077772fa12c)

# #Variable Declaration
- `const` is an augmentation of `let` in that it prevents re-assignment to a variable.
- That’s because `var` declarations are accessible anywhere within their containing function, module, namespace, or global scope.

    - **Variable capturing quirks:**
        - `setTimeout` will run a function after some number of milliseconds, but only after the for loop has stopped executing.
        - By the time the for loop has stopped executing, the value of i is 10. So each time the given function gets called, it will print out 10!

        ```javascript
        for (var i = 0; i < 10; i++) {
            setTimeout(function() { console.log(i); }, 100 * i);
        }

        // Output
        10
        10
        10
        10
        10
        10
        10
        10
        10
        10
        ```

        - A common work around is to use an **IIFE** - an Immediately Invoked Function Expression - to capture i at each iteration:

        ```javascript
        for (var i = 0; i < 10; i++) {
            // capture the current state of 'i'
            // by invoking a function with its current value
            (function(i) {
                setTimeout(function() { console.log(i); }, 100 * i);
            })(i);
        }
        ```

- **Shadowing:** Not to say that block-scoped variable can never be declared with a function-scoped variable. 
    - The block-scoped variable just needs to be declared within a distinctly different block.
    - Shadowing should usually be avoided in the interest of writing clearer code.

    ```typescript
    function f(condition, x) {
        if (condition) {
            let x = 100;
            return x;
        }

        return x;
    }

    f(false, 0); // returns '0'
    f(true, 0);  // returns '100'
    ```

- `let` declarations have drastically different behavior when declared as part of a loop. 
    - Rather than just introducing a new environment to the loop itself, these declarations sort of create a new scope per iteration. 
    - Since this is what we were doing anyway with our IIFE, we can change our old setTimeout example to just use a let declaration.

    ```javascript
    for (let i = 0; i < 10 ; i++) {
        setTimeout(function() { console.log(i); }, 100 * i);
    }

    // Output
    0
    1
    2
    3
    4
    5
    6
    7
    8
    9
    ```
- `const`: Should not be confused with the idea that the values they refer to are immutable.

```javascript
const numLivesForCat = 9;
const kitty = {
    name: "Aurora",
    numLives: numLivesForCat,
}

// Error
kitty = {
    name: "Danielle",
    numLives: numLivesForCat
};

// all "okay"
kitty.name = "Rory";
kitty.name = "Kitty";
kitty.name = "Cat";
kitty.numLives--;
```

## #Destructuring:
- Destructuring works with already-declared variables as well.

```javascript
// swap variables
[first, second] = [second, first];
```

- You can create a variable for the remaining items in a list using the syntax `...` :

```javascript
let [first, ...rest] = [1, 2, 3, 4];
console.log(first); // outputs 1
console.log(rest); // outputs [ 2, 3, 4 ]
```

- You can just ignore trailing elements you don’t care about:

```javascript
let [first] = [1, 2, 3, 4];
console.log(first); // outputs 1

let [, second, , fourth] = [1, 2, 3, 4];
console.log(second); // outputs 2
console.log(fourth); // outputs 4
```

- **Tuple destructuring:**
    - Tuples may be destructured like arrays; the destructuring variables get the types of the corresponding tuple elements:

    ```typescript
    let tuple: [number, string, boolean] = [7, "hello", true];

    let [a, b, c] = tuple; // a: number, b: string, c: boolean
    
    let [a, b, c, d] = tuple; // Error, no element at index 3
    ```

    - As with arrays, you can destructure the rest of the tuple with ..., to get a shorter tuple:

    ```typescript
    let [a, ...bc] = tuple; // bc: [string, boolean]
    
    let [a, b, c, ...d] = tuple; // d: [], the empty tuple
    ```

- **Object destructuring:**
    - You can also give different names to properties:

    ```typescript
    let o = {
        a: "foo",
        b: 12,
        c: "bar"
    };

    let { a: newName1, b: newName2 }: { newName1: string, newName2: number } = o;
    ```

    - Default values let you specify a default value in case a property is undefined:

    ```typescript
    function keepWholeObject(wholeObject: { a: string, b?: number }) {
        let { a, b = 1001 } = wholeObject;
    }
    ```

- **Function declaration:**
    - First of all, you need to remember to put the pattern before the default value.

    ```typescript
    function f({ a="", b=0 } = {}): void {
        // ...
    }

    f();
    ```

    - Give a default for optional properties on the destructured property instead of the main initializer.

    ```typescript
    function f({ a, b = 0 } = { a: "" }): void {
        // ...
    }

    f({ a: "yes" }); // ok, default b = 0
    f(); // ok, default to { a: "" }, which then defaults b = 0
    f({}); // error, 'a' is required if you supply an argument
    ```

- **Spread:**
    - Object spreading is more complex than array spreading.
        - Properties that come later in the spread object overwrite properties that come earlier.

        ```javascript
        let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
        
        let search = { food: "rich", ...defaults };
        
        console.log(defaults.food); // "rich"
        ```

    - You lose methods when you spread instances of an object.
