# How Generics Allow You to Build Reusable, Strictly Typed Functions and Components

## Introduction

One of the best features offered by TypeScript is called **Generics**. If generics were not available, developers would be forced to make a choice between writing the same logic over and over again for different data types, or else using `any`, and therefore losing all type safety. Generics provide the ability to write the logic once while maintaining strong typing.

---

## The Problem Generics Solve

Imagine you need a function that returns the first element of an array. Without generics, you might write:

```ts
function getFirstNumber(arr: number[]): number {
  return arr[0];
}

function getFirstString(arr: string[]): string {
  return arr[0];
}
```

This is repetitive. The logic is identical — only the types differ. Alternatively, you could use `any`:

```ts
function getFirst(arr: any[]): any {
  return arr[0];
}
```

It now works on all arrays, but now TypeScript does not know what the output type is. For example, calling `getFirst(["hello", "world"])` will have a return type of `any` rather than `string`.

---

## Generics: Write Once, Type Correctly Every Time

A **generic** function uses a type parameter — typically written as `<T>` — that acts as a placeholder. TypeScript fills in the actual type when the function is called.

```ts
function getFirst<T>(arr: T[]): T {
  return arr[0];
}
```

Now TypeScript infers the type automatically:

```ts
const firstNumber = getFirst([1, 2, 3]); // TypeScript knows: number
const firstString = getFirst(["a", "b"]); // TypeScript knows: string
```

One function, correct types every time — no `any`, no duplication.

---

## Generic Constraints

Sometimes you want a generic to accept only types that have certain properties. You can constrain a type parameter using `extends`.

```ts
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user = { id: 1, name: "John Doe", age: 21 };

getProperty(user, "name"); // "John Doe" — TypeScript knows this is a string
getProperty(user, "email"); // TypeScript Error: "email" doesn't exist on user
```

The constraint `K extends keyof T` guarantees that `key` must be a valid property of `obj`. TypeScript will catch invalid keys at compile time, not at runtime.

---

## Generic Interfaces

Generics aren't limited to functions. Interfaces can be generic too, making them reusable across different data shapes.

```ts
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

const userResponse: ApiResponse<{ name: string; age: number }> = {
  data: { name: "Alice", age: 25 },
  status: 200,
  message: "Success",
};

const listResponse: ApiResponse<string[]> = {
  data: ["item1", "item2"],
  status: 200,
  message: "Success",
};
```

The same `ApiResponse` interface works for any response shape, and TypeScript tracks the exact type of `data` in each case.

---

## Generic Classes

Classes can also use generics to build flexible, type-safe data structures.

```ts
class Stack<T> {
  private items: T[] = [];

  push(item: T): void {
    this.items.push(item);
  }

  pop(): T | undefined {
    return this.items.pop();
  }

  peek(): T | undefined {
    return this.items[this.items.length - 1];
  }
}

const numberStack = new Stack<number>();
numberStack.push(10);
numberStack.push(20);
numberStack.pop(); // TypeScript knows this returns number | undefined

const stringStack = new Stack<string>();
stringStack.push("hello");
stringStack.push(42); // TypeScript Error: Argument of type 'number' is not assignable to 'string'
```

---

## Conclusion

Generics lie at the very core of any reusable and type-safe TypeScript codebase. With generics, there's no more need for type duplication and replication of your logic; additionally, none of TypeScript's powerful type checking capabilities is sacrificed along the way. Regardless of whether it's an auxiliary function, a definition of a class or interface representing an API response, or anything else, generics allow expressing the idea with all types accounted for by TypeScript.
