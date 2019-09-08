---
layout: post
title: "TypeScript: Part 1"
categories: TypeScript TypeSystem BasicTypes
published: true
---
A cheatsheet on TypeScript.
Find additional TypeScript configuration Gists, [here.](https://gist.github.com/thisis-Shitanshu/4159f38d651861540ba52077772fa12c)

# #Type System

```
$ npm install -g typescript
```
Type Script is a compiler compiled language. You need two tools to work with Typecript: Editor & TypeScript Compiler.
- **Features:**
    - Errors can be caught at compile time rather than at the run time.
    - A compiler error typically tells you exactly where something went wrong and what exactly went wrong whereas a runtime error is accompanied by a stack trace that may be misleading and results on a significant amount of time spent on debug work.

- **To initiate TS in a project:**
```
$ tsc --init
```

# #Basic Types
- **Boolean:**

```typescript
let isDone: boolean = false;
```

- **Number:**
    - As in JavaScript, all numbers in TypeScript are floating point values.

```typescript
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```

- **String:**

```typescript
let color: string = "blue";
color = 'red';

// Multiple line template string.
let sentence: string = `Hello, my name is ${ fullName }.
I'll be ${ age + 1 } years old next month.`;
```

- **Array:**
    - Most common: The type of the elements followed by [].

    ```typescript
    let firstFivePrimes: number[] = [2, 3, 5, 7, 11];
    ```
    - Less common: Uses **Generic Types**
    
    ```typescript
    let firstFivePrimes2: Array<number> = [2, 3, 5, 7, 11];
    ```

- **Tuple:** 
    - Tuple types allow you to express an array with a fixed number of elements whose types are known, but need not be the same.

    ```typescript
    // Declare a tuple type
    let x: [string, number];
    // Initialize it
    x = ["hello", 10]; // OK
    // Initialize it incorrectly
    x = [10, "hello"]; // Error
    ```

    - Accessing an element outside the set of known indices fails with an error.

    ```typescript
    x[3] = "world"; // Error
    
    console.log(x[5].toString()); // Error, Property '5' does not exist on type '[string, number]'.
    ```

- **Enums:**
    - An enum is a way of giving more friendly names to sets of numeric values.

    ```typescript
    enum Color {Red, Green, Blue};
    let c: Color = Color.Green;
    ```

    - Enums begin numbering their members starting at 0.
        - You can change this by manually setting the value of one of its members.

        ```typescript
        enum Color {Red = 1, Green, Blue};
        let c: Color = Color.Green;
        ```

        - Manually set all the values in the enum.

        ```typescript
        enum Color {Red = 1, Green = 2, Blue = 4};
        let c: Color = Color.Green;
        ```

    - With enums, you can also go from a numeric value to the name of that value in the enum: Reverse Mapping

    ```typescript
    enum Color {Red = 1, Green, Blue};
    let colorName: string = Color[2];

    console.log(colorName); // Displays 'Green' as its value is 2 above
    ```

    - Alternatively enums can be initialised with string values which is a more readable approach.

    ```typescript
    enum SocialMedia {
        Facebook = 'FACEBOOK',
        Twitter = 'TWITTER',
        Instagram = 'INSTAGRAM',
        LinkedIn = 'LINKEDIN'
    }
    ```

- **Any:**
    - It gives you the power to opt-out of type checking.

    ```typescript
    let variable: any = 'a string';
    variable = 5;
    variable = false;
    variable.someRandomMethod(); /* Okay, someRandomMethod might exist at runtime. */
    ```

    - The `any` type is also handy if you know some part of the type, but perhaps not all of it.

    ```typescript
    let list: any[] = [1, true, "free"];

    list[1] = 100;
    ```

    - *You can use a combination of **typeof** & **keyof** to find the type of an unknown variable:*

    ```typescript
    const person = {
        name: 'Tod',    // string
        age: 28         // number
    };

    type Person = typeof person

    // K = "name" | "age"
    function getProp<T, K extends keyof T>(obj: T, key: K) {
        return obj[key];
    }

    // const personName: number;
    const personName = getProp(person, 'age');
    ```

- **Void:**
    - It is the absence of having any type at all.
    - It is commonly used as the return type of function that do not return a value.

    ```typescript
    function sayMyName(name: string): void {
        console.log(name);
    }
    sayMyName('Heisenberg');
    ```

    - You can only assign `null` (only if `--strictNullChecks` is not specified) or `undefined` to them.

    ```typescript
    let unusable: void = undefined;
    unusable = null; // OK if `--strictNullChecks` is not given
    ```


- **Null and Undefined:**
    - They're not extremely useful on their own but they become useful when used within union types.
        - That means you can assign `null` and `undefined` to something like `number`.

    ```typescript
    type someProp = string | null | undefined;
    ```

    - **When using the `--strictNullChecks` flag, `null` and `undefined` are only assignable to any.**

- **Never:**
    - The `never` type represents the type of values that never occur. 
        - `never` is the return type for a function expression or an arrow function expression that always throws an exception or one that never returns.
        - Variables also acquire the type `never` when narrowed by any type guards that can never be true.

        ```typescript
        // Function returning never must have unreachable end point
        function error(message: string): never {
            throw new Error(message);
        }

        // Inferred return type is never
        function fail() {
            return error("Something failed");
        }

        // Function returning never must have unreachable end point
        function infiniteLoop(): never {
            while (true) {
            }
        }
        ```

- **Object:**
    - `object` is a type that represents the non-primitive type, i.e. anything that is not `number`, `string`, `boolean`, `symbol`, `null`, or `undefined`.
    - With object type, APIs like Object.create can be better represented.

    ```typescript
    declare function create(o: object | null): void;

    create({ prop: 0 }); // OK
    create(null); // OK

    create(42); // Error
    create("string"); // Error
    create(false); // Error
    create(undefined); // Error
    ```

- **Type assertions:**
    - A type assertion is like a type cast in other languages, but performs no special checking or restructuring of data.
    - It has no runtime impact, and is used purely by the compiler.

    - Type assertions have two forms:
        - “angle-bracket” syntax:

        ```typescript
        let someValue: any = "this is a string";

        let strLength: number = (<string>someValue).length;
        ```

        - the `as`-syntax:
            - When using TypeScript with JSX, only as-style assertions are allowed.

        ```typescript
        let someValue: any = "this is a string";

        let strLength: number = (someValue as string).length;
        ```