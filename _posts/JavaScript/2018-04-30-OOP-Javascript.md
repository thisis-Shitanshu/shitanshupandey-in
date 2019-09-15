---
layout: post
title: "JavaScript Object-Oriented Programming"
categories: JavaScript OOP
published: true
---

Notes on Object-oriented Programming in JavaScript

# #Object-oriented Programming
- A programming paradigm centered around objects rather than functions.
    - Languages that support OOP: C#, Java, Ruby, Python, JavaScript.

# #4 Pillars of OOP
1. **Encapsulation:**
    - We group realted variables and functions that operate on them into objects.
    
    ```javascript
    let employee = {
        baseSalary = 30000,
        overtime: 10,
        rate: 20,
        getWage: function() {
            return this.baseSalary + (this.overtime * this.rate);
        }
    };

    employee.getWage();
    ```
    - Contradicting to functional programming in OOP, functions in an object have fewer or no parameters.
        - Thus, are not pure functions.
1. **Abstraction:**
    - Make simpler Interface
    - Reduce the Impact of Change
1. **Inheritance:**
    - Mechanism that eliminate redundant code.
1. **Polymorphism:**
    - Many Form
    - Get rid of long, if and else or switch statements to compare different types of sub-object in parent object.
    - Refactor switch/case statements.
        - Every object can have it's own custome type defined inside it.

# #Objects
- Data Type in JavaScript.

1. **Creating Objects:**
    - **Object Literal:**

        ```javascript
        const circle = {
            radius: 1,
            location: {
                x: 1,
                y: 1
            },
            draw: function() {
                console.log('draw');
            }
        };

        circle.draw();
        ```

    - **Factories Function:**
        - With a return statement.

        ```javascript
        function createCircle(radius) {
            return {
                radius,
                draw: function() {
                    console.log('draw');
                }
            }
        };

        const circle = createCircle(1);
        circle.draw();
        ```

    - **Constructor Function:**
        - JavaScript, we don't really have classes.
        - `new` automatically return `this` for the object created.

        ```javascript
        function Circle(radius) {
            this.radius,
            this.draw: function() {
                console.log('draw');
            }
        };

        const circle = new Circle(1);
        circle.draw();
        ```
        
2. **Working with Objects:**
    - **Constructor Property:**
        - Every object in JavaScript has constructor property.
            - That reference the function that was used to create the object.
        - When we create an object using Object Literal syntax, interally JavaScript uses Object constructor function.
            - `new Object();`
            - JavaScript has built-in constructors:
                - `new String();`
                - `new Boolean();`
                - `new Number();`

    - **Functions are Objects:**
        
        ```javascript
        const Circle = new Function('radius', `
        this.radius = radius;
        this.draw = function() {
            console.log('draw');
        }
        `);

        const circle = new Circle(1);
        ```

        - Methods avaiable to functions:
            - `Circle.call({}, 1);` is equal to `new Circle(1);`
            - `Circle.apply({}, [1]); // Take parameters as array.`

1. **Primitives and Reference Types:**
    - Value/ Primitive Types:
        - Example:
            - Number
            - String
            - Boolean
            - Symbol
            - undefined
            - null
        - Values are independent, even when copied.
    - Reference Types or Objects:
        - Example:
            - object
            - Function
            - Array
        - Object are stored somewhere in the memory, and the address is stored inside the variable.
            - Thus, when we copy the address or the reference is copied.

1. **Working with Properties:**
    - Objects are dynamic.
        - You can add/remove properties after creating them.
            - `circle.location = { x: 1 };` equals `circle['location'] = { x: 1 };`
            - `delete circle.location;` equals `delete circle['location'];`
    - Enumerating Properties:

    ```javascript
    // Method 1
    for (let key in circle) {
        console.log(key, circle[key]);
    }

    // Method 2
    Object.keys(circle);
    ```

    - Check for property in Object: 
        - use: `if ('radius' in circle)`

1. **Private Properties: Abstraction**
    - Hide details: Show the essentials only.
    - Define variable with let and const as local variable instead of setting them as a property with `this`.
    - **Closure** determines what variables, function will be accesable to inner function by parent function. Not to confused with scope, because scope dies after every execution of the inner function.

    - Read only properties:
    ```javascript
    function Circle(radius) {
        this.radius = radius;

        let defaultLocation = { x: 0, y: 0 };

        Object.defineProperty(this, 'defaultLocation', {
            get: function() {
                return defaultLocation;
            },
            set: function (value) {
                defaultLocation = value;
            }
        });
    }

    const circle = new Circle(10);
    circle.defaultLocation;
    circle.defaultLocation = 1;
    ```

# #Classes: The Basic
- Class is used to create one or more object by Class defination.
- You can define Instance Propeties, Instance Methods from the class defination.

```javascript
class Rectangle {
    constructor(_width, _height, _color) {
        console.log("The Rectangle is being created!");

        this.width = _width;
        this.height = _height;
        this.color = _color;
    }

    getArea () {
        return this.width * this.height;
    }

    printDescription () {
        console.log(`width: ${this.width}, height: ${this.height}, color: ${this.color}`);
    }
}

let myRectangle = new Rectangle(5, 3, "blue");
console.log(myRectangle.getArea());
myRectangle.printDescription();
```

## #Classes: Getters and Setters

```javascript
class Square {
    constructor (_width) {
        this.width = _width;
        this.height = _width;
    }

    // Getter
    get area () {
        return this.width * this.height;
    }

    // Setter
    set area (area) {
        this.width = Math.squrt(area);
        this.height = this.width;
    }
}

let square1 = new Square(4);
console.log(square1.area);

square1.area = 25;
console.log(square1.width);
console.log(square1.height);
```

## #Classes: Static Methods

```javascript
class Square {
    constructor (_width) {
        this.width = _width;
        this.height = _width;
    }

    // Static method
    static equals (a, b) {
        return a.width * a.height === a.height * a.width;
    }
}

let square1 = new Square(8);
let square2 = new Square(9);

// On class not objects
console.log(Square.equals(square1, square2)); // false
```

## #Classes: Inheritance & Extends

```javascript
class Person {
    constructor (_name, _age) {
        this.name = _name;
        this.age = _age;
    }

    describe () {
        console.log(`name ${this.name}, age ${this.age}`);
    }
}

class Programmer extends Person {
    constructor(_name, _age, _yearsOfExperience) {
        super(_name, _age);

        // Custom behaviour
        this.yearsOfExperience = _yearsOfExperience;
    }

    code () {
        console.log(`${this.name} is coding`);
    }
}

let person1 = new Perons("Jeff", 45);
let programmer1 = new Programmer("Dom", 56, 12);
```

## #Classes: Polymorphism
- Act of defining a method in the derived child class of a parent.

```javascript
class Dog extends Animal {
    constructor (name) {
        super(name);
    }

    makeSound() {
        super.makesound();
        console.log('woof! woof!');
    }
}
```