# Chapter 1 Summary: Getting to Know TypeScript

This chapter helps you understand the core mechanics of TypeScript, including how its type system works and how it differs from JavaScript.

---

## ✅ Item 1: Understand the Differences Between TypeScript and JavaScript

### Guideline:
TypeScript is a **superset** of JavaScript that adds types, but types are erased at runtime. TypeScript types exist only at compile time to catch errors early.

#### ❌ Misconception:
```ts
function greet(name: string) {
  console.log("Hello " + name.toUpperCase());
}

greet(undefined); // No runtime error in JS, but TS catches it
```

#### ✅ Takeaway:
Always remember that TypeScript doesn't enforce types at runtime. You need to add your own runtime checks if necessary.

---

## ✅ Item 2: Know Which TypeScript Options You’re Using

### Guideline:
Compiler options (`tsconfig.json`) greatly affect TypeScript's behavior. Important options:

- `strict`: Enables all strict type checks.
- `noImplicitAny`: Prevents implicit `any` types.
- `strictNullChecks`: Makes `null` and `undefined` distinct.
- `esModuleInterop`: Eases importing CommonJS modules.

#### ✅ Example:
```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true
  }
}
```

---

## ✅ Item 3: Understand That Code Generation is Independent of Types

### Guideline:
TypeScript compiles to JavaScript **regardless of type errors**. It does not block compilation when errors exist (unless you're using `noEmitOnError`).

#### ❌ Example:
```ts
const age: number = "not a number"; // Error
```

#### ✅ Solution:
```json
{
  "compilerOptions": {
    "noEmitOnError": true
  }
}
```

---

## ✅ Item 4: Get Comfortable with Structural Typing

### Guideline:
TypeScript uses **structural typing**, not nominal typing. Types are compatible if their shapes match.

#### ✅ Example:
```ts
interface Point {
  x: number;
  y: number;
}

function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}

const pointLike = { x: 12, y: 26, label: "center" };
logPoint(pointLike); // ✅ Allowed: extra properties are okay
```

---

## ✅ Item 5: Limit Use of the `any` Type

### Guideline:
Avoid `any` unless you have no alternative. It removes all type safety.

#### ❌ Dangerous:
```ts
function log(value: any) {
  console.log(value.toUpperCase()); // Might crash
}
```

#### ✅ Safer:
```ts
function log(value: unknown) {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  }
}
```

---

## ✅ Item 6: Use `unknown` Instead of `any` for Safer Input Handling

### Guideline:
Prefer `unknown` when accepting values of arbitrary types. You must **narrow** `unknown` before use.

#### ✅ Example:
```ts
function handleInput(input: unknown) {
  if (typeof input === "number") {
    console.log(input.toFixed(2));
  }
}
```

---

## ✅ Item 7: Think of Types as Sets of Values

### Guideline:
You can think of types like mathematical sets. `string` is a set of all strings; `"hello"` is a subset.

#### ✅ Example:
```ts
type Animal = "cat" | "dog";
type Pet = "dog";

const pet: Pet = "dog";
const animal: Animal = pet; // ✅ OK
```

---








## Item 6: Use Your Editor to Interrogate and Explore the Type System

Your IDE (VS Code, WebStorm, etc.) is a powerful tool for understanding TypeScript types. Hovering over variables, functions, and properties reveals their inferred types, helping you debug and learn.

```typescript
const response = fetch('https://api.example.com/data');
// Hover over 'response' → Reveals `Promise<Response>`
```

### Why it matters?
- Avoid runtime errors by catching type mismatches early.
- Discover complex types without manual debugging.

### Best Practices:
- ✔ Use Quick Fix (Ctrl+.) to resolve type errors.
- ✔ Go to Definition (F12) to explore type definitions.

---

## Item 7: Think of Types as Sets of Values

A type defines a set of possible values. Some types are broader (number), while others are narrower (42).

```typescript
type SmallNumber = 1 | 2 | 3;  // Only 1, 2, or 3
type AnyNumber = number;       // All numbers
```

### Key Insight:
- `SmallNumber` is a subset of `AnyNumber`.
- `never` = empty set, `unknown` = universal set.

### Best Practices:
- ✔ Use union types (`|`) to define precise value sets.
- ✔ Use `extends` to check subset relationships.

---

## Item 8: Know How to Tell Whether a Symbol Is in the Type Space or Value Space

TypeScript has two worlds:
- **Type Space** (compile-time types, erased at runtime).
- **Value Space** (actual JavaScript values).

```typescript
interface Person {  // Type space
  name: string;
}

const Person = {    // Value space (runtime object)
  getName() { return "Alice"; }
};

function greet(p: Person) {  // Uses type Person
  console.log(Person.getName());  // Uses value Person
}
```

### Why it matters?
- Avoid confusion between types and runtime values.
- Some constructs (e.g., class, enum) exist in both spaces.

### Best Practices:
- ✔ Prefix type-only variables with `T` (e.g., `TUser`).
- ✔ Use `typeof` to extract types from values.

---

## Item 9: Prefer Type Declarations (:) Over Type Assertions (as)

Type declarations check correctness, while assertions force TypeScript to trust you (risky!).

```typescript
// ✅ Safe (checks type)
const length: number = input.length;

// ❌ Unsafe (overrides checks)
const length = input.length as number;
```

### Why avoid `as`?
- Can hide real bugs.
- Only use when you know more than TypeScript (e.g., after runtime checks).

### Best Practices:
- ✔ Use `as` only for:
  - Working with `any` → safer types.
  - Narrowing types after validation.

---

## Item 10: Avoid Object Wrapper Types (String, Number, Boolean)

JavaScript has primitives (`string`) and object wrappers (`String`). Always use primitives.

```typescript
const good: string = "hello";  // ✅ Correct  
const bad: String = "hello";   // ❌ Avoid (rarely needed)
```

### Why it matters?
- Primitives are faster and more predictable.
- Wrapper objects can lead to weird behavior (`new String("a") !== "a"`).

### Best Practices:
- ✔ Never use `String`, `Number`, `Boolean` in type annotations.

---

## Item 11: Recognize the Limits of Excess Property Checking

TypeScript checks extra properties in object literals but not when assigned indirectly.

```typescript
interface Point { x: number; y: number; }

// ✅ Error (caught early)
const p1: Point = { x: 1, y: 2, z: 3 };

// ❌ No error (assigned via variable)
const obj = { x: 1, y: 2, z: 3 };
const p2: Point = obj;  // No error!
```

### Why it matters?
- Helps catch typos in object literals.
- But not foolproof—be careful with dynamic objects.

### Best Practices:
- ✔ Use exact types (`{ x: number } & { y: number }`) for stricter checks.

---

## Item 12: Apply Types to Entire Function Expressions

Typing whole functions (instead of parameters) improves reusability.

```typescript
// ✅ Better (reusable type)
const add: (a: number, b: number) => number = (a, b) => a + b;

// ❌ Less reusable
const add = (a: number, b: number): number => a + b;
```

### Why it matters?
- Easier to reuse function types.
- Better type inference in callbacks.

### Best Practices:
- ✔ Use `type` or `interface` for complex function types.

---

## Item 13: Know the Differences Between `type` and `interface`

Both define object shapes, but have key differences:

| Feature             | `type` | `interface` |
|---------------------|--------|-------------|
| Extends             | `&`    | `extends`   |
| Implements          | ✅     | ✅          |
| Unions (`|`)        | ✅     | ❌          |
| Declaration merging | ❌     | ✅          |

### Best Practices:
- ✔ Use `interface` for public APIs (clearer error messages).
- ✔ Use `type` for unions, tuples, or complex types.

---

## Item 14: Use Type Operations (`Pick`, `Omit`) to Avoid Repetition

Keep types DRY (Don’t Repeat Yourself) with utility types.

```typescript
interface User {
  id: string;
  name: string;
  email: string;
}

// ✅ Extract only needed fields
type UserPreview = Pick<User, 'id' | 'name'>;
```

### Why it matters?
- Reduces maintenance burden.
- Ensures consistency across types.

### Best Practices:
- ✔ Learn `Partial`, `Required`, `Readonly` for common transformations.

---

## Item 15: Use Index Signatures for Dynamic Data

When working with dictionary-like objects, use index signatures.

```typescript
const phonebook: { [name: string]: string } = {
  alice: "123-456-7890",
  bob: "987-654-3210",
};
```

### Why it matters?
- Enforces value types for dynamic keys.
- Avoids `any` misuse.

### Best Practices:
- ✔ Use `Record<KeyType, ValueType>` for cleaner syntax.

---

## Item 16: Prefer Arrays/Tuples Over Number Index Signatures

Arrays already have number-based indexing, so avoid manual `{ [index: number]: T }`.

```typescript
// ❌ Unnecessary
interface BadArray {
  [index: number]: string;
}

// ✅ Better
const names: string[] = ["Alice", "Bob"];
```

### Why it matters?
- Arrays have built-in methods (`map`, `filter`).
- Tuples (`[string, number]`) enforce fixed-length types.

### Best Practices:
- ✔ Use `Array<T>` or `T[]` for lists.
- ✔ Use tuples for fixed-length arrays (e.g., React `useState`).

---

## Item 17: Use `readonly` to Prevent Mutations

Mark properties as `readonly` to enforce immutability.

```typescript
interface Config {
  readonly apiUrl: string;
}

const config: Config = { apiUrl: "https://api.example.com" };
// config.apiUrl = "new-url";  // ❌ Error (immutable)
```

### Why it matters?
- Prevents accidental modifications.
- Makes intent clear.

### Best Practices:
- ✔ Use `Readonly<T>` for entire objects.

---

## Item 18: Use Mapped Types to Keep Values in Sync

Mapped types (`keyof`, `in`) help transform types dynamically.

```typescript
type Optional<T> = { [K in keyof T]?: T[K] };
type ReadonlyUser = Readonly<User>;
```

### Why it matters?
- Avoids manual type duplication.
- Enables advanced type patterns.

### Best Practices:
- ✔ Use `Partial`, `Required`, `Pick` for common cases.

Item 19: Avoid Cluttering Your Code with Inferable Types
TypeScript can infer types in many cases, so you don't need to explicitly annotate them.

Example:

typescript
// Unnecessary type annotation
const x: number = 12;

// Better - TypeScript infers the type
const x = 12;
Details:

TypeScript can infer types for:

Variables initialized when declared

Default function parameters

Function return types (unless you want specific documentation)

Exceptions where you should annotate:

Object literals (to catch excess properties)

Function parameters (for public APIs)

Where the type isn't immediately obvious





# Chapter 3 Summary: Type Design

This chapter helps you design better types—types that are flexible, safe, and expressive.

---

## ✅ Item 20: Use Different Variables for Different Types

### Guideline:
Don’t overload a single variable with values of different types. Instead, use **distinct variables** to keep types precise.

#### ❌ Example:
```ts
let id: string | number = "user_1";
id = 123; // allowed, but type safety is diluted
```

#### ✅ Better:
```ts
const userId: string = "user_1";
const orderId: number = 123;
```

---

## ✅ Item 21: Use `object` and `Record<string, unknown>` for Loose Types

### Guideline:
Avoid using `{}` or `any` when working with general object values.

#### ❌ Weak typing:
```ts
function handle(data: {}) {
  // cannot safely access properties
}
```

#### ✅ Better:
```ts
function handle(data: Record<string, unknown>) {
  if (typeof data.name === "string") {
    console.log(data.name);
  }
}
```

Or:
```ts
function handle(data: object) {
  // You know it's a non-primitive
}
```

---

## ✅ Item 22: Use `unknown` for Values with Unknown Types

### Guideline:
Prefer `unknown` over `any` when you don’t know the type yet.

#### ❌ Unsafe:
```ts
function logLength(input: any) {
  console.log(input.length); // Might crash
}
```

#### ✅ Safer:
```ts
function logLength(input: unknown) {
  if (typeof input === "string") {
    console.log(input.length);
  }
}
```

---

## ✅ Item 23: Create Nested Types When Needed

### Guideline:
Don't flatten complex types. Use **nested structures** to make relationships clearer.

#### ✅ Example:
```ts
interface User {
  id: string;
  profile: {
    name: string;
    birthDate: Date;
  };
}
```

---

## ✅ Item 24: Be Consistent in Your Use of Types

### Guideline:
Pick one form (e.g., `interface` or `type`, `string[]` or `Array<string>`) and **stick to it** for consistency.

#### ✅ Be consistent:
```ts
type User = {
  id: string;
  roles: string[];
};
```

#### ❌ Inconsistent:
```ts
interface User {
  id: string;
  roles: Array<string>; // Mixing styles
}
```

---

## ✅ Item 25: Prefer `readonly` for Immutable Data

### Guideline:
Use `readonly` to prevent mutations and clearly express intent.

#### ✅ Example:
```ts
interface Config {
  readonly apiUrl: string;
  readonly retries: number;
}
```

#### Array version:
```ts
const nums: readonly number[] = [1, 2, 3];
```

---

## ✅ Item 26: Use `readonly` to Document Immutability

### Guideline:
`readonly` communicates that a value shouldn't change.

#### ✅ Example:
```ts
function printConfig(config: Readonly<Config>) {
  // config.apiUrl = "http://new-api.com"; ❌ Error
}
```

---

## ✅ Item 27: Use Enums Judiciously

### Guideline:
Use `enums` only when necessary. Prefer `union types` for simpler cases.

#### ❌ Verbose enum:
```ts
enum Status {
  Success,
  Failure,
}
```

#### ✅ Simpler union:
```ts
type Status = "success" | "failure";
```

---




# Chapter 4 Summary

This chapter focuses on writing TypeScript code that is **readable**, **maintainable**, and **clear** for other developers.

---

## ✅ Item 28: Prefer Types That Always Represent Valid States

### ❌ Problematic Example
```ts
interface User {
  username: string;
  email?: string;
  phoneNumber?: string;
}
// What if both email and phoneNumber are missing?
```

### ✅ Better
```ts
type ContactInfo =
  | { kind: "email"; email: string }
  | { kind: "phone"; phoneNumber: string };

interface User {
  username: string;
  contact: ContactInfo;
}
```

Now, TypeScript ensures that the user has *either* an email *or* a phone number — but not both or neither.

---

## ✅ Item 29: Be Precise with Tuple Types

Use **tuples** when the length and types of elements are fixed and meaningful by position.

### ✅ Tuple Example
```ts
type Point = [number, number];

function distance([x1, y1]: Point, [x2, y2]: Point) {
  return Math.sqrt((x2 - x1)**2 + (y2 - y1)**2);
}
```

### ❌ Less Clear with Arrays
```ts
function distance(p1: number[], p2: number[]) { ... }
```

---

## ✅ Item 30: Use `readonly` to Avoid Mutating State

### ❌ Mutable Example
```ts
function freezePoint(p: [number, number]) {
  p[0] = 0; // oops, this mutates the original
}
```

### ✅ Immutable Example
```ts
function freezePoint(p: readonly [number, number]) {
  // p[0] = 0; // Error: cannot assign to read-only property
}
```

---

## ✅ Item 31: Prefer `Array` and `Record` over Object Type Literals

### ✅ Clearer with Utility Types
```ts
type Scores = Record<string, number>;
```

### ❌ Less Clear
```ts
type Scores = { [key: string]: number };
```

---

## ✅ Item 32: Prefer `Partial`, `Pick`, and `Omit` to Writing Object Types from Scratch

```ts
interface User {
  id: string;
  name: string;
  email: string;
}

// ✅ Update user with Partial
type UserUpdate = Partial<User>; // All fields optional

// ✅ Pick only a few fields
type UserPreview = Pick<User, 'name' | 'email'>;

// ✅ Omit certain fields
type UserWithoutEmail = Omit<User, 'email'>;
```

---

## ✅ Item 33: Use `ReturnType` to Type Functions That Return Functions

```ts
function makeAdder(x: number) {
  return (y: number) => x + y;
}

type Adder = ReturnType<typeof makeAdder>; // (y: number) => number
```

---

## Summary of Chapter 4 Principles

| Concept | Key Idea |
|--------|----------|
| Valid State Types | Design types so that invalid states are impossible |
| Tuples | Use for fixed-length, positionally meaningful data |
| `readonly` | Prevent accidental mutations |
| Utility Types (`Record`, `Array`) | Clearer and more expressive than raw object types |
| `Partial`, `Pick`, `Omit` | Avoid duplication when transforming types |
| `ReturnType` | Reuse function types without retyping |












# Chapter 5 – Working with `any`, `unknown`, and `never`

---

### 🔹 Item 43: Prefer `unknown` to `any`

**Summary**:  
If you must use a top type (a type that can hold any value), prefer `unknown` over `any`. `unknown` forces you to do some type checking before operating on the value, providing more safety than `any`.

✅ **Good**:
```ts
function parseJson(jsonStr: string): unknown {
  return JSON.parse(jsonStr);
}

const result = parseJson('{"name": "Omar"}');
if (typeof result === 'object' && result !== null && 'name' in result) {
  console.log((result as { name: string }).name);
}
```

❌ **Bad**:
```ts
function parseJsonUnsafe(jsonStr: string): any {
  return JSON.parse(jsonStr);
}

const user = parseJsonUnsafe('{"name": "Omar"}');
console.log(user.toUpperCase()); // Runtime error if user is not a string!
```

🧠 **Takeaway**: `unknown` encourages safer code than `any`.

---

### 🔹 Item 44: Understand the differences between `unknown` and `any`

**Summary**:  
`unknown` is safer: it requires type narrowing or assertions before usage. `any` disables type checking completely.

Example:
```ts
let valueAny: any = "hello";
let valueUnknown: unknown = "hello";

console.log(valueAny.trim());       // OK, but might crash
// console.log(valueUnknown.trim()); // ❌ Error

if (typeof valueUnknown === 'string') {
  console.log(valueUnknown.trim()); // ✅ Safe
}
```

🧠 **Takeaway**: Use `unknown` to preserve type safety with dynamic values.

---

### 🔹 Item 45: Prefer more precise variants of `any` when possible

**Summary**:  
When you need a flexible type, try using:
- `unknown`
- `object`
- `Record<string, unknown>`
- Custom types

Example:

❌ **Too generic**:
```ts
function handleInput(input: any) {
  // unsafe
}
```

✅ **Better**:
```ts
function handleInput(input: Record<string, unknown>) {
  if (typeof input.name === 'string') {
    console.log(input.name.toUpperCase());
  }
}
```

🧠 **Takeaway**: Use more descriptive types instead of `any`.

---

### 🔹 Item 46: Understand that `any` erases type safety

**Summary**:  
Using `any` silences all type errors — even obvious ones.

Example:
```ts
function unsafeAdd(a: any, b: any): any {
  return a + b;
}

unsafeAdd(1, 2); // OK
unsafeAdd('a', 2); // OK (but probably wrong)
unsafeAdd({}, []); // OK (but meaningless)
```

🧠 **Takeaway**: `any` should be your last resort.

---

### 🔹 Item 47: Use `never` for exhaustive checks

**Summary**:  
`never` represents a value that should never occur. It’s useful in exhaustive `switch` statements.

Example:
```ts
type Shape = { kind: 'circle', radius: number } | { kind: 'square', size: number };

function getArea(shape: Shape): number {
  switch (shape.kind) {
    case 'circle': return Math.PI * shape.radius ** 2;
    case 'square': return shape.size ** 2;
    default:
      const _exhaustive: never = shape;
      return _exhaustive;
  }
}
```

🧠 **Takeaway**: Use `never` to make your code future-proof.

---

### 🔹 Item 48: Recognize that some uses of `never` are error-prone

**Summary**:  
Beware of using `never` in type aliases or utility types incorrectly — it can lead to surprising behavior.

Example:
```ts
type Keys = 'a' | 'b';
type Filter<T> = T extends 'a' ? never : T;

type Result = Filter<Keys>; // Result is 'b'

type ArrayFilter<T> = T extends 'a' ? never : T;
type Filtered = ArrayFilter<Keys>; // 'b'
type FilteredArray = Array<Filtered>; // Array<'b'>
```


---











# Chapter 6: Generics and Type-Level Programming

---

## Item 50: Think of Generics as Functions Between Types

Generics behave like functions operating on types.

```ts
type Box<T> = { value: T };
type StringBox = Box<string>; // { value: string }
```

---

## Item 51: Avoid Unnecessary Type Parameters

Simplify your types by avoiding unused generic parameters.

Bad:
```ts
function identity<T>(arg: T): T {
  return arg;
}
```

Better (when T is always number):
```ts
function identity(arg: number): number {
  return arg;
}
```

---

## Item 52: Prefer Conditional Types to Overload Signatures

Use conditional types to reduce overload complexity.

```ts
type ElementType<T> = T extends (infer U)[] ? U : T;

type A = ElementType<string[]>; // string
type B = ElementType<number>;   // number
```

---

## Item 53: Know How to Control the Distribution of Unions over Conditional Types

Conditional types distribute over unions:

```ts
type ToArray<T> = T extends any ? T[] : never;
type A = ToArray<string | number>; // string[] | number[]
```

Prevent distribution using a tuple:

```ts
type ToArrayNonDist<T> = [T] extends [any] ? T[] : never;
type B = ToArrayNonDist<string | number>; // (string | number)[]
```

---

## Item 54: Use Template Literal Types to Model DSLs and Relationships Between Strings

Create string patterns with template literals:

```ts
type EventName = `on${Capitalize<string>}`;
```

---

## Item 55: Write Tests for Your Types

Assert type correctness with helper utilities:

```ts
type Assert<T extends true> = T;
type IsString<T> = T extends string ? true : false;

type Test = Assert<IsString<'hello'>>; // Passes
type Fail = Assert<IsString<42>>;      // Error
```

---

## Item 56: Pay Attention to How Types Display

Simplify complex types for readability:

```ts
type Simplify<T> = { [K in keyof T]: T[K] };
```

---

## Item 57: Prefer Tail-Recursive Generic Types

Avoid compiler limits by using tail recursion:

```ts
type Flatten<T, Acc extends any[] = []> = T extends [infer Head, ...infer Tail]
  ? Flatten<Tail, [...Acc, Head]>
  : Acc;
```

---

## Item 58: Consider Code Generation as an Alternative to Complex Types

When types are overly complex, automate their generation with code to reduce errors and improve maintainability.

---












# Chapter 7: Writing and Running Your Code

---

## Item 72: Prefer ECMAScript Features to TypeScript Features

TypeScript has introduced features like enums, parameter properties, and namespaces. However, many of these have been adopted or superseded by ECMAScript standards. It's advisable to use standard ECMAScript features when possible for better compatibility and clarity.

### Examples

- **Enums**

  Instead of using TypeScript enums:

  ```typescript
  enum Direction {
    Up,
    Down,
    Left,
    Right
  }
  ```

  Prefer union types:

  ```typescript
  type Direction = 'Up' | 'Down' | 'Left' | 'Right';
  ```

- **Parameter Properties**

  Instead of:

  ```typescript
  class Point {
    constructor(public x: number, public y: number) {}
  }
  ```

  Prefer explicit property declarations:

  ```typescript
  class Point {
    public x: number;
    public y: number;

    constructor(x: number, y: number) {
      this.x = x;
      this.y = y;
    }
  }
  ```

- **Namespaces and Triple-Slash Imports**

  Avoid using namespaces and triple-slash directives (`/// <reference path="..." />`). Instead, use ES6 module syntax:

  ```typescript
  import { MyModule } from './my-module';
  ```

- **Decorators**

  Decorators are still experimental and not part of the ECMAScript standard. Use them cautiously and only when necessary, such as in frameworks like Angular.

---

## Item 73: Use Source Maps to Debug TypeScript

Source maps allow developers to debug TypeScript code directly in browsers by mapping the compiled JavaScript back to the original TypeScript source.

Make sure source maps are enabled in your `tsconfig.json`:

```json
{
  "compilerOptions": {
    "sourceMap": true
  }
}
```

This lets you set breakpoints and inspect variables in your TypeScript code during runtime, making debugging easier.

---

## Item 74: Know How to Reconstruct Types at Runtime

TypeScript's type system is erased during compilation, so types are not available at runtime.

To perform type checks at runtime, use JavaScript constructs like `typeof` and `instanceof`, or implement custom type guards.

### Example:

```typescript
function isString(value: unknown): value is string {
  return typeof value === 'string';
}

const value: unknown = 'hello';
if (isString(value)) {
  // TypeScript now knows 'value' is a string
  console.log(value.toUpperCase());
}
```

---

## Item 75: Understand the DOM Hierarchy

When working with the DOM, understand the hierarchy of interfaces TypeScript provides.

For example, `HTMLElement` is a superclass for specific element types like `HTMLDivElement` or `HTMLInputElement`.

### Example:

```typescript
const input = document.querySelector('input');
if (input instanceof HTMLInputElement) {
  input.value = 'Hello';
}
```

Using `instanceof` ensures TypeScript knows the specific type of the DOM element, allowing access to its properties.

---

## Item 76: Create an Accurate Model of Your Environment

TypeScript relies on type declarations to understand the shape of objects and APIs.

When working with environments like Node.js or browser APIs, ensure you include the appropriate type declarations.

For example, install type definitions for Node.js:

```bash
npm install --save-dev @types/node
```

This helps TypeScript provide accurate type checking and IntelliSense.

---

## Item 77: Understand the Relationship Between Type Checking and Unit Testing

TypeScript's type system helps catch many errors at compile time but doesn't eliminate the need for unit testing.

Types ensure functions are used correctly but don't verify that functions produce correct results.

### Example:

```typescript
function add(a: number, b: number): number {
  return a - b; // Oops, should be a + b
}
```

TypeScript won't catch this logical error because the types are correct. Unit tests are necessary to validate function behavior.

---

## Item 78: Pay Attention to Compiler Performance

As TypeScript projects grow, compilation times can increase.

To maintain performance:

- Use the `incremental` compiler option to enable faster subsequent builds:

  ```json
  {
    "compilerOptions": {
      "incremental": true
    }
  }
  ```

- Exclude unnecessary files from compilation using the `exclude` option in `tsconfig.json`.

- Consider using project references for large codebases to enable faster builds and better organization.

---


