# Understanding some features in TypeScript

TypeScript is the superset of JavaScript. It has gained most popularity among developers for its ability to add static typing to JavaScript.

---

## Difference between Interfaces and Types:

Interfaces and types in TypeScript seem interchangeable. Both allow us to define the shape of an object, but some differences make them suitable for different use cases. Here's a breakdown of their key differences:

### 1. **Declaration Merging**

One of the most significant differences is that **interfaces support declaration merging**, while **types do not**. Declaration merging allows us to define multiple declarations of the same interface, and TypeScript will automatically combine them into one.

#### Example:

```typescript
interface User {
  name: string;
}

interface User {
  age: number;
}

const user: User = {
  name: "Alice",
  age: 25,
};
```

In this example, the `User` interface is merged, combining both `name` and `age` properties. This is particularly useful in scenarios like extending third-party libraries.

On the other hand, types do not support merging:

```typescript
type User = {
  name: string;
};

// Error: Duplicate identifier 'User'
type User = {
  age: number;
};
```

---

### 2. **Unions, intersections, and primitive types**

While interfaces are primarily designed for object shapes, but types are more versatile. They can represent unions, intersections, and even primitive types.

#### Example:

Using a union with types:

```typescript
type Status = "success" | "error" | "loading";
```

Using an intersection with types:

```typescript
type User = {
  name: string;
} & {
  age: number;
};
```

Interfaces do not natively support unions or intersections.

---

### 3. **Extending or Implementing**

Both interfaces and types can be extended, but the syntax are different.

Using interfaces:

```typescript
interface Animal {
  species: string;
}

interface Dog extends Animal {
  breed: string;
}
```

Using types:

```typescript
type Animal = {
  species: string;
};

type Dog = Animal & {
  breed: string;
};
```

---

### 4. **Performance**

Internally, TypeScript treats interfaces more efficiently than types in certain cases, such as error reporting. This is usually negligible, interfaces might be slightly faster to compile in large codebases.

## When to Use Interfaces and Types

- **Use interfaces** when we need to define the shape of objects and take advantage of declaration merging.
- **Use types** when we need unions, intersections, or more complex type compositions.

---

### Quick Summary

| Feature                  | Interface          | Type               |
| ------------------------ | ------------------ | ------------------ |
| Declaration Merging      | ✅ Supported       | ❌ Not Supported   |
| Union and Intersection   | ❌ Not Supported   | ✅ Supported       |
| Primarily for Objects    | ✅ Yes             | ✅ Yes             |
| Performance Optimization | ✅ Slightly Better | ❌ Slightly Slower |

---

## `keyof` Keyword: A TypeScript Superpower

The `keyof` keyword in TypeScript is a powerful utility that allows us to create a type based on the keys of an object. This enables dynamic and type-safe programming.

---

### How Does `keyof` Work?

The `keyof` operator takes an object type and produces a union of its keys as string literal types.

#### Example:

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

type UserKeys = keyof User;
// Equivalent to: "id" | "name" | "email"
```

In this example, the `UserKeys` type is a union of `"id"`, `"name"`, and `"email"`, that is the keys of the `User` type.

---

### Use Case of `keyof`

The `keyof` keyword is often used to enforce type-safe property access in functions.

#### Example:

```typescript
function processValue(value: string | number): number {
  if (typeof value === "string") {
    return value.length;
  } else {
    return value * 2;
  }
}

processValue("hello"); // Output: 5
processValue(10); // Output: 20
```

In this example, the `processValue` function uses the `typeof` operator to check the type of the `value` parameter. If it's a string, it returns its length. If it's a number, it returns its double.

---

## When to Use `keyof`

- **Use `keyof`** when we need to enforce type-safe property access or dynamically create types based on object keys.

---

## The `any` Type

The `any` type is the most flexible type in TypeScript. It allows us to assign any value to a variable without any compile-time checks.

### Characteristics:

- Using `any` disables static type checking.
- We can perform any operation on an `any` type variable.
- Overusing `any` defeats the purpose of TypeScript and can lead to runtime errors.

### Example:

```typescript
let data: any;
data = "Hello"; // Valid
data = 42; // Valid
console.log(data.toFixed(2)); // No error, even though `toFixed` is not safe.
```

---

## The `unknown` Type

The `unknown` type is a safer alternative to `any`. It allows us to store any value but enforces type checking before performing operations on it. This makes it a more type-safe choice for handling dynamic or uncertain data.

### Characteristics:

- We can narrow the type of an `unknown` variable before using it.
- Prevents unintended operations or errors.

### Example:

```typescript
let data: unknown;
data = "Hello";
data = 42;

if (typeof data === "number") {
  console.log(data.toFixed(2)); // Safe
}
```

### Key Difference: `any` vs. `unknown`

- **`any`:** We can perform any operation without restrictions.
- **`unknown`:** We must check the type before performing an operation.

---

## The `never` Type

The `never` type represents values that never occur. It is often used to indicate:

1. A function that never returns (e.g., it throws an error or runs indefinitely).
2. Code that cannot be reached.

### Characteristics:

- A `never` type variable cannot hold any value.
- Indicates Unreachable Code.

### Example:

```typescript
function throwError(message: string): never {
  throw new Error(message);
}
```

---

## Enums in TypeScript

An enum (short for "enumeration") is a special type that represents a set of named constants. TypeScript supports two types of enums: **numeric enums** and **string enums**, but we can also use **mixed enums** in TypeScript.

---

### Numeric Enums

Numeric enums assign automatic or manual numeric values to their members.

#### Example:

```typescript
enum Direction {
  Up, // 0
  Down, // 1
  Left, // 2
  Right = 10, // 10
}

console.log(Direction.Up); // Output: 0
console.log(Direction.Right); // Output: 10
```

---

### String Enums

String enums assign string literals to their members, making them more readable.

#### Example:

```typescript
enum Status {
  Success = "SUCCESS",
  Error = "ERROR",
  Pending = "PENDING",
}

console.log(Status.Success); // Output: "SUCCESS"
console.log(Status.Error); // Output: "ERROR"
```

---

### Mixed enums

TypeScript allow us to combine both numeric and string values within the same enum. Typically, enums are either fully numeric or fully string-based, but TypeScript lets us mix them when needed.

```typescript
enum MixedEnum {
  NumericValue = 1,
  StringValue = "string",
}
```

---

## When to Use Enums

Enums are particularly useful when we need a group of related constant values that:

1. Represent a fixed set of possible choices (e.g., directions, statuses, roles).
2. Improve code readability and type safety by replacing magic numbers or strings.

---

## Conclusion

TypeScript offers a rich set of features to make our code more robust and maintainable. Understanding the differences between interfaces and types, mastering the `keyof` keyword, and effectively using `any`, `unknown`, `never`, and enums can significantly enhance our TypeScript skills.

Happy coding!
