---
layout: post
title: "Functional Programming in JavaScript"
categories: JavaScript FunctionalProgramming
published: true
---

Functional Programming in JS: What? Why? How?

# #What is?
- A coding style suported by some languages.
    - E.g., F#, Haskell, Erlang, Javascript, Scala.
- Programming paradigm:
    1. Imperative:
        - follow my commands: do this, then that
    1. Object-Oriented:
        - keep state to yourself: send/recieve message.
    1. Declarative:
        - this is what I want: I don't care how you do it.
        - E.g., SQL
    1. **Functional:**
        - Click [here](https://codewords.recurse.com/issues/one/an-introduction-to-functional-programming).
        - Use pure function:
            - A function that only depends on it's input arguments.
            - Returns an output, instead of a side effect.
            - When called with the same arguments should always return the same output.

# #Why is functional programming important?
- Pure functions are more predictable and safe.
- Easier to test/debug.

# #Principles:
1. Do everything with functions.
1. Avoid side effects:
    - Do nothing but return output.

    ```javascript
    // With side effects
    let conf = { name: "SaintJS", date: 2017 };

    function renameConf(newName) {
        conf.name = newName;
        console.log("Renamed!");
    }

    renameConf("HolyJS"); // Renamed!
    conf; // {name: "HolyJS", date: 2017}

    // Pure function
    let conf = { name: "SaintJS", date: 2017 };

    function renameConf(oldConf, newName) {
        return { name: newName, date: oldConf.date }
    }

    let newConf = renameConf(conf, "HolyJS");
    newConf; // { name: "HolyJS", date: 2017 }
    conf; // { name: "SaintJS", date: 2017 }
    ```
1. Avoid mutability:
    - Don't change in-place; instead, replace a new.
    - But it is not very efficient.
        - Use **Persistent Data Structures**, for efficiently handling immutable data.
            - Instead of representing an array as regular array, represent it as a tree.
            - So if you want to change something, you don't have to change the whole structure.
                - Just copy a small subset of the DS.
                - Copy any node pointing to the node, you just changed, and so on.
            - This added **Structural Sharing.**
    - *JavaScript doesn't have this build-in. Arrays and Objets are not imutable. But there's a lot of libraries.*
        - Mori
            - Large but performant, better for Back-end.
        - Immutable.js:
            - By facebook. Click [here](https://immutable-js.github.io/immutable-js/).
            - Small, better for Front-end.

1. Don't iterate:
    - Use recurse.

    ```javascript
    // Iteration
    function sum (numbers) {
        let total = 0;
        for (i = 0; i < numbers.length; i++) {
            total += numbers[i];
        }
        return total;
    }

    sum([0, 1, 2, 3, 4]); // 10
    
    // Recurse
    function sum(numbers) {
        if(numbers.length === 1) {
            // Base case
            return numbers[0];
        } else {
            // Recusive case
            return numbers[0] + sum(numbers.slice(1));
        }
    }

    sum([0, 1, 2, 3, 4]); // 10
    ```

1. Don't loop:
    - Use map, reduce, filter.
        - filter: returns an array, to remove unwanted individual elements for certain condition.
        - map: returns an array, to process individual elements and give a new values for each elements.
        - reduce: returns a single value, for the combination of array.

1. Use Higher-Order fucntions:
    - Functions with functions as inputs/outputs.
    - Only for the languages that supports first class functions.
        - Passing functions as variables.

    ```javascript
    // Closures

    function makeAdjectifier(adjective) {
        return function (noun) {
            return adjective + " " + noun;
        };
    }

    var holify = makeAdjective("holy");
    holify("JS"); // "holy JS"
    holify("cow"); // "holy cow"
    ```

1. Flow data through functions:
    - outputs become to next function inputs to next function become outputs to next function.

    ```javascript
    let ender = (ending) => (input) => input + ending;

    let adore = ender("rocks");
    let announce = ender(", y'all");
    let exclaim = ender("!");

    var hypeUp = (x) => exclaim(announc(adore(x)));
    hypeUp("JS"); // JS rocks, y'all!

    // With library
    let r = require("ramda");

    let rtlHype = r.compose(adore, annunce, exclaim);
    rtlHype("FP"); // "FP!, y'all rocks"

    let ltrHype = r.pipe(adore, annunce, exclaim);
    ltrHype("FP"); // "FP rocks, y'all!"
    ```

# #FP libraies for JS
- Mori
- Immutable
- Ramda
- Underscore
- Lodash