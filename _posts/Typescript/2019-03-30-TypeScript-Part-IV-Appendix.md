---
layout: post
title: "TypeScript: Part IV"
categories: TypeScript 3.0 TypeAssertion TypeCompatibility TypeGuard LiteralTypes ConditionalTypes GenericTypes MappedTypes
published: true
---

@v3.0.0

## #Types
- Unknown **(Introduced in 3.0)**
    - It is the type-safe counterpart of **any**.
    - Anything is assignable to **unknown**, but **unknown** isn't assignable to anything but itself and **any**.
    - No operations are permitted on an **unknown** without first asserting or narrowing to a more specific type.
```typescript
type I1 = unknown & null; // null
type I2 = unknown & string; // string
type U1 = unknown | null; // unknown
type U2 = unknown | string; // unknown
```

- Type Alias
    - They provide names for type annotations allowing you to use it in several places.
```typescript
type Login = string;
```

- Union Type
    - TypeScript allows us to use more than one data type for a property. This is called a union type.
```typescript
 type Password = string | number;
```

- Intersection Type
    - These are the types that combine properties of all the member types.

    ```typescript
    interface Person {
        name: string;
        age: number;
    }

    interface Worker {
        companyId: string;
    }

    // Here!
    type Employee = Person & Worker;

    const bestOfTheMonth: Employee = {
        name: 'Peter',
        age: 39,
        companyId: '123456'
    }
    ```

# #Type Assertion
Typescript allows overriding its inferred types in any way.
This is used when having a better understanding of a variable type than the compiler on its own.
- Type assertion is usually used when migrating code from JavaScript.
- Below when `age` property is missing, the compiler might help in providing autocomplete for properties of Person but it will not complain if you miss any properties.

```typescript
const friend = {};
friend.name = 'John'; // Error! Property 'name' does not exist on type '{}'

interface Person {
    name: string;
    age: number;
}

const person = {} as Person;
person.name = 'John' // Okay
```


# #Type Inference
TypeScript infers type of variable when there is no explicit information avaiable in the form of type annotations.
```typescript
/**
* Variable definitinon
*/
let a = "some string";
let b = 1;
a = b; // Error! Type 'number' is not assignable to type 'string'.

/**
* Function return types
*/
function sum(x: number, y: number) {
    return x + y; // Here!!! infer to return a number
}
```


# #Type Compatibility
Type compatibility is based on structural typing, which relates types based solely on their members.
The basic rule for the structural type is that `x` is compatible with `y`, if `y` has at least the same members as `x`.
```typescript
interface Person {
    name: string;
}

let x: Person; // Okay, despite not being an implementation of the Person interface
let y = { name: 'John', age: 20 }; // type { name: string; age: number }
x = y;

// Please note that x is still of type Person. 
// In the following example, the compiler will show an error message as it does not
// expect the property age in Person but the result will be as expected:
console.log(x.age); // 20
```

- Functions
    - Number of arguments
        - In a function call you need to pass in at least enough arguments, meaning that extra arguments will not case any errors.

        ```typescript
        function consoleName(person: Person) {
            console.log(person.name);
        }

        consoleName({ name: 'John' });
        consoleName({ name: 'John', age: 30 }); // Extra argument still Okay
        ```
    - Return type
        - The return type must contain at least enough data.
        ```typescript
        let x = () => ({name: 'John'});
        let y = () => ({name: 'John', age: 20 });
        x = y; // OK
        y = x; /* Error! Property 'age' is missing in type '{ name: string; }'
        but required in type '{ name: string; age: number; }' */
        ```


# #Type Guard
Type Guards allows to narrow down the type of an object within a conditional block.

- typeof
    - Using typeof in a conditional block, the cmpiler will know the type of a variable to be different outside the conditional block.

```typescript
function example(x: number | boolean) {
    if (typeof x === 'number') {
        return x.toFixed(2);
    }

    return x.ToFixed(2); // // Error! Property 'toFixed' does not exist on type 'boolean'.
}
```

- instanceof

```typescript
class MyResponse {
    header = 'header example';
    result = 'result example';
    // ...
}

class MyError {
    header = 'header example';
    message = 'message example';
    // ...
}

function example(x: MyResponse | MyError) {
    if(x instanceof MyResponse) {
        console.log(x.message); // Error! Property 'message' does not exist on type 'MyResponse'.
        console.log(x.result);
    } else {
        // TypeScript knows this must be MyError

        console.log(x.message);
        console.log(x.result); // Error! Property 'result' does not exist on type 'MyError'.
    }
}
```

- in
    - The `in` operator checks for the existence of a property on an object.


# #Literal Types
Literals are exact values that are JavaScript primitives. They can be combined in a type union to create useful abstractions.
```typescript
type Orientation = 'lanscape' | 'portrait';
function changeOrientation(x: Orientation) {
    // ...
}
changeOrientation('portrait'); // Okay
changeOrientation('vertical'); /* Error! Argument of type '"vertical"' is not assignable to parameter of type 'Orientation'. */
```

# #Conditional Types
A conditional type describes a typical relationship test and selects one of two possible types, depending on the outcome of that test.
```typescript
    type X = A extends B ? C : D;
```


# #Generic Types
These must include or reference another type in order to be complete. It enforce meaningful constraints between various variables.
- The following function returns an array of whatever type you pass in.

```typescript
function reverse<T>(items: T[]): T[] {
    return items.reverse();
}
reverse([1, 2, 3]); // number[]
reverse([0, true]); // (number | boolean)[]
```

- keyof
    - This operator queries the set of keys for a given type.

```typescript
interface Person {
    name: string;
    age: number;
}

type PersonKeys = keyof Person; // 'name' | 'age'
```


## #Mapped Types
- It allows creating new types from existing ones by mapping over property types.
- Each property of the existing type is transformed according to a rule that you specify.

- Partial:
```typescript
type Partial<T> = {
    [P in keyof T]?: T[P];
};
```

- Readonly:
    - The `Readonly` type that takes a type `T` and sets all of its properties as readonly.  
```typescript
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};
```

- Exclude
    - `Exclude` allows to remove certain types from another type.
    - `Exclude` from `T` anything that is assignable to `T`.

```typescript
// type Exclude<T, U> = T extends U ? never : T;
type User = {
    _id: number;
    name: string;
    email: string;
    created: number;
};

type UserNoMeta = Exclude<keyof User, '_id' | 'created'>
```

- Pick
    - It allows to pick ceratin types from another type.
    - `Pick` from `T` anything that is assignable to `T`.

```typescript
/*
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
};
*/

type UserNoMeta = Pick<User, 'name' | 'email'>
```

- infer
    - One can use the **infer** keyword to infer a type variable within the **extends** clause of a conditional type.
    - Such an inferred type variable can only be used in the true branch of the conditional type.

- ReturnType

```typescript
/**
* Original TypeScript's ReturnType
* type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;
*/

type MyReturnType<T> = T extends (...args: any) => infer R ? R : any;

type TypeFromInfer = MyReturnType<() => number>; // number
type TypeFromFallback = MyReturnType<string>; // any
```