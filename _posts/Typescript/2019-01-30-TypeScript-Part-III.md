---
layout: post
title: "TypeScript: Part III"
categories: TypeScript Interfaces Classes Function
published: true
---

A cheatsheet on TypeScript.
Find additional TypeScript configuration Gists, [here.](https://gist.github.com/thisis-Shitanshu/4159f38d651861540ba52077772fa12c)

# #Interface
Interface specify in a single named annotation exactly what properties to expect with its respective type annotations.
- They have ZERO runtime JS impact.
- Optional properties are declared with `?`. 
    - Meaning, that the object of the interface may or may not define these properties.
- A `read-only` property means, once a property is assigned a value, it cannot be changed.

```typescript
interface ICircle {
    readonly id: string;
    center: {
        x: number;
        y: number;
    };
    radius: number;
    color?: string; // Optional property
}

const circle1: ICircle = {
    id: '001',
    center: { x: 0 },
    radius: 8
}; /* Error! Property 'y' is missing in type '{ x: number; }' but required in type '{ x: number; y: number; }'. */

const circle2: ICircle = {
    id: '002',
    center: { x: 0, y: 0 },
    radius: 8
}; // Okay

circle2.color = '#666'; // Okay
circle2.id = '003'; /* Error! Cannot assign to 'id' because it is a read-only property. */
```
- Function In Interface:

```typescript
interface ICircleWithParameter {
    getParameter(): number;
}
```

- Extending Interface
    - Interface can extend one or more interfaces.

```typescript
interface ICircleWithArea extends ICircle {
    getArea: () => number;
}

const circle3: ICircleWithArea = {
    id: '003',
    center: { x: 0, y: 0 },
    radius: 6,
    color: '#fff',
    getArea: function () {
        return (this.radius ** 2) * Math.PI;
    },
};
```

- Implementing an Interface
    - A class implementing an interface needs to strictly conform to the structure of the interface.

```typescript
interface IClock {
    currentTime: Date;
    setTime(d: Date): void;
}

class Clock implements IClock {
    currentTime: Date = new Date();
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
}
```

# #Classes
One can add types to properties and method's arguments.
```typescript
class Greeter {
    greeting: string;
    constructor(message: string): void {
        this.greeting = message;
    }
    greet(name: string): string {
        return `Hi ${name}, ${this.greeting}`;
    }
}
```

- Access Modifiers
    - Typescript supports `public`, `private`, `protected` modifiers, which determine the accessibility of class member.

```
 | Accessible on    |public | protected | private   |
 | :------------:   | :----:| :-------: | :-----:   |
 | w/in class       | yes   |    yes    |    yes    |
 | class children   | yes   |    yes    |    no     |
 | class instance   | yes   |    no     |    no     |
```

- Readonly modifier
    - They must be initialised at their declaration or in the constructor.

```typescript
class Spider {
    readonly name: string;
    readonly numberOfLegs: number = 8;
    constructor (theName: string) {
        this.name = theName;
    }
}
```

- Parameter properties
    - They let the creation and initialization of a member in one place.
    - They are declared by prefixing a constructor parameter with a modifier.

```typescript
class Spider {
    readonly numberOfLegs: number = 8;
    constructor(readonly name: string) {}
}
```

## #Abstract
The abstract keyword can be used both for classes and for abstract class methods.
- `Abstract classes` cannot be directly instantiated. They are mainly for inheritance where the class which extends the abstract class must define all the abstract methods.
- `Abstract members` do not contain an implementation, thus they cannot be directly accessed. These members must be implemented in child classes.


# #Functions
```typescript
function add(x: number, y: number) {
    return x + y;
}
```

- Function Overloads
    - TypeScript allows you to declare function overloads.
    - To create a function overload, you just declare the function header multiple times.

```typescript
function padding(all: number);
function padding(topAndBottom: number, leftAndRight: number);
function padding(top: number, right: number, bottom: number, left: number);

function padding(a: number, b?: number, c?: number, d?: any): any {
    if(b === undefined && c === undefined && d === undefined) {
        b = c = d = a;
    }
    else if ( c === undefined && d === undefined ) {
        c = a;
        d = d;
    }

    return {
        top: a,
        right: b,
        bottom: c,
        left: d
    };
}

padding(1); // Okay
padding(1, 1); // Okay
padding(1, 1, 1, 1); // Okay
padding(1, 1, 1); /* Error! No overload expects 3 arguments, but
overloads do exist that expect either 2 or 4 arguments. */
```