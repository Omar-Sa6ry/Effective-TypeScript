TypeScript is structural
Type compatibility is based on structure, not names. If two types have the same shape, they’re considered compatible—even if they have different names.

Types only exist at compile time
TypeScript types are erased at runtime. You can’t check custom types with typeof or instanceof—those only work with real JS types like number, string, etc.

Trust the compiler, but verify
TypeScript’s inference is strong, but it can be too broad or too narrow. Be explicit when necessary, especially in public-facing APIs.

Use type inference, but be deliberate
Let the compiler infer types when it makes sense, but step in and annotate when clarity or precision is important.

Enable --strict mode and --noImplicitAny
These compiler options make your code safer by enforcing better type discipline.

Prefer unknown over any
any disables type safety. unknown requires you to check the type before using it, which leads to safer code.

Use const and readonly
Immutability helps TypeScript give you better type inference and reduces bugs. Favor const over let and use readonly in your types where possible.













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
