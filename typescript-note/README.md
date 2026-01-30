# Complete TypeScript Guide for Frontend Developers: From Basics to Production

## Table of Contents
1. [Part 1: What is TypeScript and Why It Matters](#part-1-what-is-typescript-and-why-it-matters)
2. [Part 2: Setting Up Your Environment](#part-2-setting-up-your-environment)
3. [Part 3: Basic Types](#part-3-basic-types)
4. [Part 4: Functions and Type Safety](#part-4-functions-and-type-safety)
5. [Part 5: Objects and Interfaces](#part-5-objects-and-interfaces)
6. [Part 6: Advanced Types](#part-6-advanced-types)
7. [Part 7: Generics](#part-7-generics)
8. [Part 8: Real-World Patterns](#part-8-real-world-patterns)
9. [Part 9: React & Next.js with TypeScript](#part-9-react--nextjs-with-typescript)
10. [Part 10: Best Practices & Production Patterns](#part-10-best-practices--production-patterns)
11. [Mini-Projects](#mini-projects)

---

## Part 1: What is TypeScript and Why It Matters

### Understanding the Problem TypeScript Solves

JavaScript is **dynamically typed**, which means variables can be any type:

```javascript
let x = 5;
x = "hello";  // ✓ Valid in JavaScript, but confusing
x.toUpperCase();  // This works, but what if x was still 5? Error!
```

**TypeScript adds static typing** — you declare what type something should be, and the compiler catches mistakes **before** they reach production.

```typescript
let x: number = 5;
x = "hello";  // ✗ Error caught immediately
x.toUpperCase();  // ✗ Error: numbers don't have toUpperCase()
```

### Why Frontend Developers Need TypeScript

- **Catch bugs early**: Prevents runtime errors in production
- **Better IDE support**: Auto-completion and refactoring
- **Self-documenting code**: Types tell other developers (or future you) what data is expected
- **Easier refactoring**: Change a type once, the compiler tells you everywhere it breaks
- **Industry standard**: React, Vue, Angular teams all use TypeScript
- **Scalability**: Managing large codebases becomes exponentially easier

### TypeScript in the Real World

TypeScript is NOT a runtime language—it compiles to JavaScript. Your browser still runs JavaScript, but TypeScript catches errors during development.

```
TypeScript Code (.ts) → Compiler → JavaScript Code (.js) → Browser
```

---

## Part 2: Setting Up Your Environment

### Quick Start

For learning purposes, use **TypeScript Playground**: https://www.typescriptlang.org/play

For a real project:

```bash
# Create a new project
npx create-react-app my-app --template typescript

# Or with Next.js
npx create-next-app@latest --typescript

# Or manually set up TypeScript
npm install --save-dev typescript
npx tsc --init  # Creates tsconfig.json
```

### Key Configuration: tsconfig.json

After `npx tsc --init`, your `tsconfig.json` looks like:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "lib": ["ES2020", "DOM"],
    "strict": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src"]
}
```

**Most important**: Set `"strict": true` for maximum type safety.

---

## Part 3: Basic Types

### The Primitive Types

```typescript
// String
let name: string = "Alice";
let message: string = `Hello, ${name}`;

// Number (includes integers, floats, NaN, Infinity)
let age: number = 25;
let price: number = 19.99;

// Boolean
let isActive: boolean = true;
let hasErrors: boolean = false;

// Null and Undefined
let nothing: null = null;
let notDefined: undefined = undefined;

// Any (AVOID THIS - defeats the purpose of TypeScript)
let mystery: any = 5;
mystery.toUpperCase();  // TypeScript won't complain, but it crashes at runtime
```

### Arrays

```typescript
// Array of strings
let colors: string[] = ["red", "green", "blue"];

// Alternative syntax
let numbers: Array<number> = [1, 2, 3];

// Array of mixed types (avoid when possible)
let mixed: (string | number)[] = ["hello", 42, "world"];

// Readonly arrays (cannot be modified)
let fixed: readonly string[] = ["a", "b"];
fixed.push("c");  // ✗ Error
```

### Literal Types (Specific Values)

```typescript
// Specifies EXACTLY which values are allowed
let status: "pending" | "active" | "inactive" = "active";
status = "pending";  // ✓ OK
status = "deleted";  // ✗ Error

// Number literals
let httpStatus: 200 | 404 | 500 = 200;
```

### Type Aliases (Creating Reusable Types)

```typescript
// Creating a type that represents a specific shape
type UserRole = "admin" | "user" | "guest";

let myRole: UserRole = "admin";  // ✓ OK
let theirRole: UserRole = "superuser";  // ✗ Error

// More complex type
type Point = {
  x: number;
  y: number;
};

let position: Point = { x: 10, y: 20 };  // ✓ OK
let badPosition: Point = { x: 10 };  // ✗ Error: missing y
```

### Union Types (Multiple Possibilities)

```typescript
// Variable can be string OR number
let id: string | number;
id = "USER-123";  // ✓ OK
id = 456;        // ✓ OK
id = true;       // ✗ Error

// Union with more types
type Status = string | number | boolean;
```

### Intersection Types (Combining Types)

```typescript
type Admin = {
  role: "admin";
  permissions: string[];
};

type User = {
  id: number;
  name: string;
};

// AdminUser must have properties from BOTH types
type AdminUser = Admin & User;

const admin: AdminUser = {
  role: "admin",
  permissions: ["read", "write", "delete"],
  id: 1,
  name: "Alice"
};
```

### Exercise 1: Basic Types

```typescript
// 1. Create a variable that can only be 1, 2, or 3
let dice: 1 | 2 | 3;

// 2. Create an array of strings
let fruits: string[];

// 3. Create a Point type with x and y properties
type Point = {
  x: number;
  y: number;
};

// 4. Create a Status type that can be "loading", "success", or "error"
type Status = "loading" | "success" | "error";
```

---

## Part 4: Functions and Type Safety

### Function Parameters and Return Types

```typescript
// Function that explicitly types parameters and return value
function add(a: number, b: number): number {
  return a + b;
}

add(5, 10);      // ✓ OK, returns 15
add("5", "10");  // ✗ Error

// Function with string parameter
function greet(name: string): string {
  return `Hello, ${name}!`;
}

// Function with no return value
function logError(message: string): void {
  console.error(message);
}

// Function that can return nothing
function maybeDoSomething(): void | undefined {
  if (Math.random() > 0.5) {
    return;
  }
}
```

### Optional Parameters and Default Values

```typescript
// Optional parameter (can be undefined)
function greetOptional(name?: string): string {
  return `Hello, ${name || "Guest"}!`;
}

greetOptional();         // ✓ OK
greetOptional("Alice");  // ✓ OK

// Default parameter
function createUser(name: string, role: string = "user"): string {
  return `${name} is a ${role}`;
}

createUser("Bob");              // ✓ "Bob is a user"
createUser("Alice", "admin");   // ✓ "Alice is a admin"
```

### Rest Parameters and Spread

```typescript
// Function that accepts multiple numbers
function sum(...numbers: number[]): number {
  return numbers.reduce((a, b) => a + b, 0);
}

sum(1, 2, 3, 4, 5);  // ✓ OK
```

### Overloading (Different Behaviors for Different Input Types)

```typescript
// Define what the function should do in different cases
function getId(input: string): string;
function getId(input: number): number;

// Implementation that handles both cases
function getId(input: string | number): string | number {
  if (typeof input === "string") {
    return input.toUpperCase();
  }
  return input * 2;
}

getId("hello");  // ✓ "HELLO"
getId(5);        // ✓ 10
```

### Arrow Functions

```typescript
// All of these are valid
const multiply = (a: number, b: number): number => a * b;

const getName = (user: { name: string }): string => user.name;

// Multiple lines
const complexOperation = (x: number): number => {
  const doubled = x * 2;
  return doubled + 1;
};
```

### Callback Functions

```typescript
// Function that takes another function as a parameter
function processArray(
  items: number[],
  callback: (item: number) => void
): void {
  items.forEach(item => callback(item));
}

processArray([1, 2, 3], (num) => console.log(num * 2));

// Return a function
function createMultiplier(factor: number): (x: number) => number {
  return (x: number) => x * factor;
}

const double = createMultiplier(2);
double(5);  // ✓ 10
```

### Exercise 2: Functions

```typescript
// 1. Write a function that takes a name and returns a greeting
function greet(name: string): string {
  // Your code here
}

// 2. Write a function that takes optional email
function signUp(name: string, email?: string): void {
  // Your code here
}

// 3. Write a function that can format either a string or number
function format(value: string | number): string {
  // Your code here
}
```

---

## Part 5: Objects and Interfaces

### What are Interfaces?

An interface is a **contract** that describes what properties and methods an object must have.

```typescript
// Define what a User object should look like
interface User {
  id: number;
  name: string;
  email: string;
  isActive: boolean;
}

// Any object matching this interface is valid
const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  isActive: true
};

// Missing property = error
const badUser: User = {
  id: 2,
  name: "Bob"
  // ✗ Error: missing email and isActive
};
```

### Interface vs Type Alias

Both can do similar things, but have subtle differences:

```typescript
// Type alias - more flexible, can use unions
type Status = "active" | "inactive";

// Interface - designed specifically for objects
interface Config {
  apiUrl: string;
  timeout: number;
}

// Use interfaces for object contracts, types for unions/primitives
// For most cases, interfaces are preferred for objects
```

### Optional Properties

```typescript
interface UserProfile {
  name: string;
  email: string;
  phone?: string;  // Optional - may or may not exist
  bio?: string;
}

const profile1: UserProfile = {
  name: "Alice",
  email: "alice@example.com"
  // ✓ OK - phone and bio not required
};

const profile2: UserProfile = {
  name: "Bob",
  email: "bob@example.com",
  phone: "+1-555-1234",
  bio: "A cool person"
};
```

### Readonly Properties

```typescript
interface ApiKey {
  readonly key: string;
  readonly createdAt: Date;
  expiresAt: Date;  // Can be modified
}

const apiKey: ApiKey = {
  key: "secret123",
  createdAt: new Date(),
  expiresAt: new Date()
};

apiKey.expiresAt = new Date("2025-12-31");  // ✓ OK
apiKey.key = "newSecret";  // ✗ Error: readonly
```

### Methods in Interfaces

```typescript
interface Calculator {
  add(a: number, b: number): number;
  subtract(a: number, b: number): number;
}

const myCalc: Calculator = {
  add: (a, b) => a + b,
  subtract: (a, b) => a - b
};

myCalc.add(5, 3);  // ✓ 8
```

### Extending Interfaces (Inheritance)

```typescript
interface Animal {
  name: string;
  age: number;
}

// Dog has everything Animal has, plus more
interface Dog extends Animal {
  breed: string;
  bark(): void;
}

const dog: Dog = {
  name: "Rex",
  age: 3,
  breed: "Labrador",
  bark: () => console.log("Woof!")
};
```

### Index Signatures (Dynamic Properties)

```typescript
// Object can have any string property with number values
interface NumberDictionary {
  [key: string]: number;
}

const scores: NumberDictionary = {
  alice: 95,
  bob: 87,
  charlie: 92
};

// Object can have specific properties plus any others
interface User {
  name: string;
  [key: string]: string | number;  // Any other properties
}

const user: User = {
  name: "Alice",
  age: 25,
  country: "USA",
  phone: 5551234567
};
```

### Exercise 3: Objects and Interfaces

```typescript
// 1. Create a Book interface with title, author, and pages
interface Book {
  // Your code here
}

// 2. Create an Author interface with name and books (array of Book)
interface Author {
  // Your code here
}

// 3. Create a ShoppingCart interface with items (Product array) and total price
interface Product {
  // Your code here
}

interface ShoppingCart {
  // Your code here
}
```

---

## Part 6: Advanced Types

### Union Types (Revisited with Type Guards)

```typescript
type Status = "loading" | "success" | "error";

function handleStatus(status: Status): string {
  switch (status) {
    case "loading":
      return "Loading...";
    case "success":
      return "Success!";
    case "error":
      return "Error occurred";
  }
}

// Discriminated Union - when objects have a common literal property
type Response = 
  | { type: "success"; data: string }
  | { type: "error"; error: string }
  | { type: "loading" };

function handleResponse(response: Response): void {
  if (response.type === "success") {
    console.log(response.data);  // TypeScript knows data exists here
  } else if (response.type === "error") {
    console.log(response.error);  // TypeScript knows error exists here
  }
}
```

### Type Guards

```typescript
// typeof guard
function processValue(value: string | number): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase());  // string method
  } else {
    console.log(value.toFixed(2));  // number method
  }
}

// instanceof guard
class Dog {
  bark() { }
}
class Cat {
  meow() { }
}

function makeSound(pet: Dog | Cat): void {
  if (pet instanceof Dog) {
    pet.bark();
  } else {
    pet.meow();
  }
}

// Custom type guard using type predicate
function isString(value: unknown): value is string {
  return typeof value === "string";
}

const mystery: unknown = "hello";
if (isString(mystery)) {
  console.log(mystery.toUpperCase());  // TypeScript knows it's a string
}
```

### Conditional Types (Advanced)

```typescript
// If T is a string, return string, otherwise return number
type Flatten<T> = T extends Array<infer U> ? U : T;

type Str = Flatten<string[]>;  // string
type Num = Flatten<number>;    // number
```

### Utility Types

TypeScript provides built-in helper types:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

// Partial - all properties become optional
type PartialUser = Partial<User>;
// Equivalent to: { id?: number; name?: string; email?: string }

// Required - all properties become required
type RequiredUser = Required<PartialUser>;

// Pick - select specific properties
type UserPreview = Pick<User, "name" | "email">;
// Equivalent to: { name: string; email: string }

// Omit - exclude specific properties
type UserWithoutId = Omit<User, "id">;
// Equivalent to: { name: string; email: string }

// Record - create an object with specific keys
type UserRole = "admin" | "user" | "guest";
type Permissions = Record<UserRole, string[]>;
// Equivalent to: { admin: string[]; user: string[]; guest: string[] }

const permissions: Permissions = {
  admin: ["read", "write", "delete"],
  user: ["read", "write"],
  guest: ["read"]
};

// Readonly - make all properties readonly
type ReadonlyUser = Readonly<User>;

// NonNullable - removes null and undefined
type NotNull = NonNullable<string | null | undefined>;  // string

// ReturnType - extracts return type of a function
function getUser(): { id: number; name: string } {
  return { id: 1, name: "Alice" };
}
type UserType = ReturnType<typeof getUser>;  // { id: number; name: string }

// Awaited - extracts type from a Promise
type Data = Awaited<Promise<{ id: number }>>;  // { id: number }
```

### Mapped Types (Creating Types from Existing Types)

```typescript
// Create a new type where all properties are readonly and optional
interface User {
  id: number;
  name: string;
  email: string;
}

type ReadonlyPartialUser = {
  readonly [K in keyof User]?: User[K];
};
// Equivalent to: { readonly id?: number; readonly name?: string; readonly email?: string }

// Practical example: create a setter object for each property
type Setters<T> = {
  [K in keyof T]: (value: T[K]) => void;
};

type UserSetters = Setters<User>;
// Equivalent to: 
// {
//   id: (value: number) => void;
//   name: (value: string) => void;
//   email: (value: string) => void;
// }
```

### Keyof Operator

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

type UserKeys = keyof User;  // "id" | "name" | "email"

// Useful for type-safe property access
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user: User = { id: 1, name: "Alice", email: "alice@example.com" };
const name = getProperty(user, "name");  // ✓ OK
const age = getProperty(user, "age");    // ✗ Error: "age" not a property
```

### Exercise 4: Advanced Types

```typescript
// 1. Create a discriminated union for API responses
type ApiResponse = 
  // Your code here

// 2. Use utility types to create a readonly version of User
type ReadonlyUser = 
  // Your code here

// 3. Create a mapped type that turns all properties into functions that return that type
type Getters<T> = 
  // Your code here
```

---

## Part 7: Generics

Generics allow you to write reusable code that works with **any type** while maintaining type safety.

### Basic Generic Syntax

```typescript
// Function that returns the same type it receives
function identity<T>(value: T): T {
  return value;
}

identity("hello");    // T is string, returns string
identity(42);         // T is number, returns number
identity(true);       // T is boolean, returns boolean

// Multiple type parameters
function swap<T, U>(a: T, b: U): [U, T] {
  return [b, a];
}

swap("hello", 42);  // Returns [42, "hello"]
```

### Generic Interfaces

```typescript
// Generic container that can hold any type
interface Box<T> {
  value: T;
  getValue(): T;
}

const stringBox: Box<string> = {
  value: "hello",
  getValue() { return this.value; }
};

const numberBox: Box<number> = {
  value: 42,
  getValue() { return this.value; }
};

// Array is a generic type!
let strings: Array<string> = ["a", "b", "c"];
let numbers: Array<number> = [1, 2, 3];
```

### Generic Arrays

```typescript
// Function that works with any array
function getFirstItem<T>(items: T[]): T | undefined {
  return items[0];
}

getFirstItem([1, 2, 3]);        // Returns number | undefined
getFirstItem(["a", "b", "c"]);  // Returns string | undefined

// Filter that maintains types
function filterArray<T>(items: T[], predicate: (item: T) => boolean): T[] {
  return items.filter(predicate);
}

filterArray([1, 2, 3, 4], n => n > 2);      // Returns number[]
filterArray(["hi", "bye"], s => s.length > 2);  // Returns string[]
```

### Generic Classes

```typescript
class Repository<T> {
  private items: T[] = [];

  add(item: T): void {
    this.items.push(item);
  }

  getAll(): T[] {
    return this.items;
  }

  getById(id: number): T | undefined {
    return this.items[id];
  }
}

interface User {
  id: number;
  name: string;
}

const userRepo = new Repository<User>();
userRepo.add({ id: 1, name: "Alice" });
userRepo.add({ id: 2, name: "Bob" });

const user = userRepo.getAll()[0];  // TypeScript knows this is User
```

### Constrained Generics

```typescript
// T must have a length property
function getLength<T extends { length: number }>(value: T): number {
  return value.length;
}

getLength("hello");      // ✓ strings have length
getLength([1, 2, 3]);    // ✓ arrays have length
getLength(42);           // ✗ Error: numbers don't have length

// T must extend a specific type
function merge<T extends object>(obj: T, patch: Partial<T>): T {
  return { ...obj, ...patch };
}

const user = { name: "Alice", age: 25 };
const updated = merge(user, { age: 26 });  // ✓ OK

// T must be a key of U
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const myUser = { id: 1, name: "Alice" };
getProperty(myUser, "name");   // ✓ OK
getProperty(myUser, "email");  // ✗ Error: email not a property
```

### Default Generic Types

```typescript
interface Response<T = string> {
  status: number;
  data: T;
}

const stringResponse: Response = {
  status: 200,
  data: "success"
};

const numberResponse: Response<number> = {
  status: 200,
  data: 42
};
```

### Practical Generic Examples

```typescript
// API call handler
interface ApiResponse<T> {
  success: boolean;
  data?: T;
  error?: string;
}

async function fetchUser<T>(id: number): Promise<ApiResponse<T>> {
  try {
    const response = await fetch(`/api/users/${id}`);
    const data = await response.json();
    return { success: true, data };
  } catch (error) {
    return { success: false, error: String(error) };
  }
}

interface User {
  id: number;
  name: string;
}

const result = await fetchUser<User>(1);
if (result.success && result.data) {
  console.log(result.data.name);  // TypeScript knows data is User
}

// Async operation wrapper
function withLoading<T>(
  fn: () => Promise<T>
): () => Promise<{ loading: boolean; data: T | null; error: Error | null }> {
  return async () => {
    try {
      const data = await fn();
      return { loading: false, data, error: null };
    } catch (error) {
      return { loading: false, data: null, error: error as Error };
    }
  };
}
```

### Exercise 5: Generics

```typescript
// 1. Create a generic cache that stores key-value pairs
class Cache<T> {
  // Your code here
}

// 2. Create a generic function that transforms an array
function map<T, U>(items: T[], transform: (item: T) => U): U[] {
  // Your code here
}

// 3. Create a generic function that merges two objects
function mergeObjects<T, U>(obj1: T, obj2: U): T & U {
  // Your code here
}
```

---

## Part 8: Real-World Patterns

### Enums (Useful for Fixed Sets of Values)

```typescript
// Numeric enum
enum UserRole {
  Guest = 0,
  User = 1,
  Admin = 2
}

function hasPermission(role: UserRole): boolean {
  return role === UserRole.Admin;
}

hasPermission(UserRole.Admin);  // ✓ true

// String enum (more readable)
enum Status {
  Pending = "pending",
  Active = "active",
  Inactive = "inactive"
}

const currentStatus: Status = Status.Active;

// Avoid enums for type-only use cases - use literal types instead
type Role = "guest" | "user" | "admin";  // Preferred for frontend
```

### Assertion Functions

```typescript
// Function that guarantees a value is of a specific type
function assertIsString(value: unknown): asserts value is string {
  if (typeof value !== "string") {
    throw new Error("Not a string");
  }
}

const mystery: unknown = "hello";
assertIsString(mystery);
console.log(mystery.toUpperCase());  // TypeScript knows it's a string

// Useful for API responses
function assertResponseOk<T>(
  response: ApiResponse<T>
): asserts response is ApiResponse<T> & { data: T } {
  if (!response.success || !response.data) {
    throw new Error(response.error || "Unknown error");
  }
}
```

### Dependency Injection Pattern

```typescript
interface Logger {
  log(message: string): void;
}

class UserService {
  constructor(private logger: Logger) {}

  createUser(name: string): void {
    this.logger.log(`Creating user: ${name}`);
    // Create user...
  }
}

// Different implementations
class ConsoleLogger implements Logger {
  log(message: string): void {
    console.log(message);
  }
}

class FileLogger implements Logger {
  log(message: string): void {
    // Write to file...
  }
}

// Inject different implementations
const consoleService = new UserService(new ConsoleLogger());
const fileService = new UserService(new FileLogger());
```

### Builder Pattern

```typescript
interface UserConfig {
  id: number;
  name: string;
  email?: string;
  role?: "admin" | "user" | "guest";
  isActive?: boolean;
}

class UserBuilder {
  private config: UserConfig;

  constructor(id: number, name: string) {
    this.config = { id, name };
  }

  withEmail(email: string): this {
    this.config.email = email;
    return this;  // Return this to allow chaining
  }

  withRole(role: "admin" | "user" | "guest"): this {
    this.config.role = role;
    return this;
  }

  withActive(isActive: boolean): this {
    this.config.isActive = isActive;
    return this;
  }

  build(): UserConfig {
    return this.config;
  }
}

// Usage
const user = new UserBuilder(1, "Alice")
  .withEmail("alice@example.com")
  .withRole("admin")
  .withActive(true)
  .build();
```

### Factory Pattern with Generics

```typescript
interface Creator<T> {
  create(): T;
}

class UserFactory implements Creator<User> {
  create(): User {
    return { id: 1, name: "Default", email: "default@example.com" };
  }
}

class ProductFactory implements Creator<Product> {
  create(): Product {
    return { id: 1, name: "Default Product", price: 0 };
  }
}

// Generic factory wrapper
function createInstance<T>(factory: Creator<T>): T {
  return factory.create();
}

const user = createInstance(new UserFactory());
const product = createInstance(new ProductFactory());
```

### Options/Config Object Pattern

```typescript
interface ButtonOptions {
  label: string;
  onClick?: () => void;
  disabled?: boolean;
  variant?: "primary" | "secondary" | "danger";
  size?: "small" | "medium" | "large";
}

function Button(options: ButtonOptions): void {
  const {
    label,
    onClick = () => {},
    disabled = false,
    variant = "primary",
    size = "medium"
  } = options;

  console.log(`${label} (${size} ${variant})`);
}

// Usage with type safety
Button({
  label: "Click me",
  onClick: () => console.log("clicked"),
  variant: "primary"
});

// Better: extend with required properties
interface RequiredButtonOptions extends ButtonOptions {
  label: string;  // Make explicitly required
  onClick: () => void;
}
```

### Observer Pattern

```typescript
interface Observer<T> {
  update(data: T): void;
}

class Subject<T> {
  private observers: Observer<T>[] = [];

  subscribe(observer: Observer<T>): void {
    this.observers.push(observer);
  }

  notify(data: T): void {
    this.observers.forEach(observer => observer.update(data));
  }
}

interface UserData {
  id: number;
  name: string;
}

class UserNotifier implements Observer<UserData> {
  update(data: UserData): void {
    console.log(`User updated: ${data.name}`);
  }
}

const subject = new Subject<UserData>();
subject.subscribe(new UserNotifier());
subject.notify({ id: 1, name: "Alice" });
```

---

## Part 9: React & Next.js with TypeScript

### Functional Components

```typescript
import { FC, ReactNode } from "react";

// Method 1: Using FC type
interface ButtonProps {
  label: string;
  onClick: () => void;
  disabled?: boolean;
}

const Button: FC<ButtonProps> = ({ label, onClick, disabled }) => {
  return (
    <button onClick={onClick} disabled={disabled}>
      {label}
    </button>
  );
};

// Method 2: Simple approach (recommended in modern React)
interface CardProps {
  title: string;
  children: ReactNode;
}

function Card({ title, children }: CardProps) {
  return (
    <div className="card">
      <h2>{title}</h2>
      {children}
    </div>
  );
}
```

### Hooks with TypeScript

```typescript
import { useState, useCallback, useRef } from "react";

// useState with TypeScript
const [count, setCount] = useState<number>(0);
const [name, setName] = useState<string>("");

// Inferred types (usually don't need explicit types)
const [users, setUsers] = useState([]);  // any[]
const [users, setUsers] = useState<User[]>([]);  // User[]

// useCallback
interface HandleClickProps {
  id: number;
  name: string;
}

const handleClick = useCallback<(props: HandleClickProps) => void>(
  ({ id, name }) => {
    console.log(`Clicked: ${name}`);
  },
  []
);

// useRef
const inputRef = useRef<HTMLInputElement>(null);

const focusInput = () => {
  inputRef.current?.focus();  // Optional chaining
};
```

### Form Handling

```typescript
import { FormEvent } from "react";

interface FormData {
  email: string;
  password: string;
}

export function LoginForm() {
  const [formData, setFormData] = useState<FormData>({
    email: "",
    password: ""
  });

  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = event.currentTarget;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    console.log("Form submitted", formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        name="email"
        value={formData.email}
        onChange={handleChange}
      />
      <input
        type="password"
        name="password"
        value={formData.password}
        onChange={handleChange}
      />
      <button type="submit">Login</button>
    </form>
  );
}
```

### Event Types

```typescript
import { MouseEvent, KeyboardEvent, ChangeEvent } from "react";

// Mouse events
const handleMouseClick = (event: MouseEvent<HTMLButtonElement>) => {
  console.log(event.currentTarget.id);
};

const handleMouseMove = (event: MouseEvent<HTMLDivElement>) => {
  console.log(event.clientX, event.clientY);
};

// Keyboard events
const handleKeyDown = (event: KeyboardEvent<HTMLInputElement>) => {
  if (event.key === "Enter") {
    event.currentTarget.blur();
  }
};

// Change events
const handleInputChange = (event: ChangeEvent<HTMLInputElement>) => {
  const value = event.currentTarget.value;
};
```

### Custom Hooks with TypeScript

```typescript
import { useState, useCallback } from "react";

// Generic custom hook
function useFetch<T>(url: string) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useCallback(async () => {
    try {
      const response = await fetch(url);
      const result: T = await response.json();
      setData(result);
    } catch (err) {
      setError(err as Error);
    } finally {
      setLoading(false);
    }
  }, [url]);

  return { data, loading, error };
}

// Usage
interface User {
  id: number;
  name: string;
}

function UserProfile() {
  const { data: user, loading } = useFetch<User>("/api/user");

  if (loading) return <div>Loading...</div>;
  return <div>{user?.name}</div>;
}
```

### Context API with TypeScript

```typescript
import { createContext, useContext, ReactNode } from "react";

interface User {
  id: number;
  name: string;
}

interface AuthContextType {
  user: User | null;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export function AuthProvider({ children }: { children: ReactNode }) {
  const [user, setUser] = useState<User | null>(null);

  const login = async (email: string, password: string) => {
    // Login logic
  };

  const logout = () => {
    setUser(null);
  };

  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error("useAuth must be used within AuthProvider");
  }
  return context;
}
```

### Next.js Pages and API Routes

```typescript
// pages/posts/[id].tsx
import { GetStaticProps, GetStaticPaths } from "next";

interface Post {
  id: number;
  title: string;
  content: string;
}

interface PostPageProps {
  post: Post;
}

export default function PostPage({ post }: PostPageProps) {
  return <div>{post.title}</div>;
}

export const getStaticPaths: GetStaticPaths = async () => {
  const posts = await fetchPosts();
  return {
    paths: posts.map(post => ({ params: { id: String(post.id) } })),
    fallback: "blocking"
  };
};

export const getStaticProps: GetStaticProps<PostPageProps> = async (context) => {
  const id = context.params?.id as string;
  const post = await fetchPost(id);

  return {
    props: { post },
    revalidate: 3600
  };
};

// pages/api/posts.ts
import { NextApiRequest, NextApiResponse } from "next";

interface ApiResponse<T> {
  success: boolean;
  data?: T;
  error?: string;
}

interface PostData {
  id: number;
  title: string;
}

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ApiResponse<PostData[]>>
) {
  try {
    const posts = await getPosts();
    res.status(200).json({ success: true, data: posts });
  } catch (error) {
    res.status(500).json({ success: false, error: "Failed to fetch posts" });
  }
}
```

### Component Prop Patterns

```typescript
import { ReactNode } from "react";

// Extend HTML element props
interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  loading?: boolean;
}

function Button({ loading, children, ...props }: ButtonProps) {
  return (
    <button {...props} disabled={loading || props.disabled}>
      {loading ? "Loading..." : children}
    </button>
  );
}

// Discriminated union for different component states
type InputProps = 
  | { variant: "text"; defaultValue?: string }
  | { variant: "number"; min?: number; max?: number }
  | { variant: "email"; type: "email" };

function Input(props: InputProps) {
  switch (props.variant) {
    case "text":
      return <input type="text" defaultValue={props.defaultValue} />;
    case "number":
      return <input type="number" min={props.min} max={props.max} />;
    case "email":
      return <input type={props.type} />;
  }
}
```

---

## Part 10: Best Practices & Production Patterns

### 1. Avoid `any` at All Costs

```typescript
// ✗ Bad
function process(data: any): any {
  return data.toUpperCase();  // Could crash
}

// ✓ Good - be specific
function process(data: string | undefined): string | null {
  return data?.toUpperCase() ?? null;
}

// If you truly don't know the type, use unknown
function process(data: unknown): string | null {
  if (typeof data === "string") {
    return data.toUpperCase();
  }
  return null;
}
```

### 2. Use Const Assertions for Literals

```typescript
// ✗ Type becomes string (too broad)
const colors = ["red", "green", "blue"];
// Type: string[]

// ✓ Type becomes specific literal union
const colors = ["red", "green", "blue"] as const;
// Type: ("red" | "green" | "blue")[]

// Useful for strict props
const status = "active" as const;  // "active", not string
```

### 3. Type-Safe Event Handling

```typescript
// ✓ Good pattern for form handlers
type FormHandler = (data: FormData) => void;
type ChangeHandler = (event: ChangeEvent<HTMLInputElement>) => void;

// ✓ Use specific event types, not synthetic events
const handleClick: React.MouseEventHandler<HTMLButtonElement> = (event) => {
  // Event is properly typed
};
```

### 4. API Response Handling

```typescript
// Create a consistent response structure
interface ApiResponse<T = unknown> {
  success: boolean;
  status: number;
  data?: T;
  error?: {
    message: string;
    code: string;
  };
}

// Generic fetch wrapper
async function apiCall<T>(
  url: string,
  options?: RequestInit
): Promise<ApiResponse<T>> {
  try {
    const response = await fetch(url, options);
    const body = await response.json();

    if (!response.ok) {
      return {
        success: false,
        status: response.status,
        error: body.error
      };
    }

    return {
      success: true,
      status: response.status,
      data: body.data
    };
  } catch (error) {
    return {
      success: false,
      status: 0,
      error: { message: "Network error", code: "NETWORK_ERROR" }
    };
  }
}

// Usage
const response = await apiCall<User>("/api/user");
if (response.success && response.data) {
  console.log(response.data.name);  // Type-safe
}
```

### 5. Error Handling Pattern

```typescript
class AppError extends Error {
  constructor(
    public message: string,
    public code: string,
    public statusCode: number = 500
  ) {
    super(message);
  }
}

function isAppError(error: unknown): error is AppError {
  return error instanceof AppError;
}

async function handleUserCreation(userData: UserData) {
  try {
    const user = await createUser(userData);
    return user;
  } catch (error) {
    if (isAppError(error)) {
      // Handle app-specific errors
      console.error(`${error.code}: ${error.message}`);
    } else {
      console.error("Unknown error:", error);
    }
    throw error;
  }
}
```

### 6. Sealed Objects Pattern

```typescript
// Prevent accidental typos in object keys
const userDefaults = {
  role: "user",
  isActive: true,
  theme: "light"
} as const;

type UserDefaults = typeof userDefaults;

// This ensures type safety
const getUserDefault = <K extends keyof UserDefaults>(key: K): UserDefaults[K] => {
  return userDefaults[key];
};

getUserDefault("role");      // ✓ "user"
getUserDefault("invalid");   // ✗ Error
```

### 7. Exhaustiveness Checking

```typescript
type Status = "pending" | "success" | "error";

function handleStatus(status: Status): string {
  switch (status) {
    case "pending":
      return "Loading...";
    case "success":
      return "Success!";
    case "error":
      return "Error";
    // If you add a new status and forget to handle it, TypeScript will error here
    default:
      const exhaustive: never = status;  // Forces you to handle all cases
      return exhaustive;
  }
}
```

### 8. Module Exports Pattern

```typescript
// ✓ Export types alongside implementations
export interface UserRepository {
  getUser(id: number): Promise<User>;
  createUser(data: CreateUserData): Promise<User>;
}

export class UserRepositoryImpl implements UserRepository {
  async getUser(id: number): Promise<User> {
    // Implementation
  }

  async createUser(data: CreateUserData): Promise<User> {
    // Implementation
  }
}

export type { User, CreateUserData };  // Re-export types for consumers
```

### 9. Strict tsconfig.json Settings

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "lib": ["ES2020", "DOM"],
    
    // Maximum strictness
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "strictBindCallApply": true,
    "strictPropertyInitialization": true,
    "noImplicitThis": true,
    "alwaysStrict": true,
    
    // Additional checks
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitOverride": true,
    
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true
  }
}
```

### 10. Performance: Type Generation from Runtime Values

```typescript
// ✗ Maintain types separately from values (duplicated)
interface User {
  id: number;
  name: string;
  email: string;
}

const defaultUser = {
  id: 0,
  name: "",
  email: ""
};

// ✓ Generate types from runtime values
const userDefaults = {
  id: 0,
  name: "",
  email: ""
} as const;

type User = typeof userDefaults;

// Now if you add a field to userDefaults, the type updates automatically
```

---

## Mini-Projects

### Project 1: Todo Application

A complete todo app demonstrating interfaces, state management, and filtering.

```typescript
// types.ts
export interface TodoItem {
  id: number;
  text: string;
  completed: boolean;
  createdAt: Date;
}

export type TodoFilter = "all" | "active" | "completed";

// todoService.ts
interface TodoService {
  addTodo(text: string): TodoItem;
  removeTodo(id: number): void;
  toggleTodo(id: number): void;
  getTodos(filter: TodoFilter): TodoItem[];
}

export class TodoServiceImpl implements TodoService {
  private todos: TodoItem[] = [];
  private nextId = 1;

  addTodo(text: string): TodoItem {
    const todo: TodoItem = {
      id: this.nextId++,
      text,
      completed: false,
      createdAt: new Date()
    };
    this.todos.push(todo);
    return todo;
  }

  removeTodo(id: number): void {
    this.todos = this.todos.filter(t => t.id !== id);
  }

  toggleTodo(id: number): void {
    const todo = this.todos.find(t => t.id === id);
    if (todo) {
      todo.completed = !todo.completed;
    }
  }

  getTodos(filter: TodoFilter): TodoItem[] {
    switch (filter) {
      case "active":
        return this.todos.filter(t => !t.completed);
      case "completed":
        return this.todos.filter(t => t.completed);
      case "all":
      default:
        return this.todos;
    }
  }
}

// React Component
import React, { useState } from "react";
import { TodoItem, TodoFilter } from "./types";
import { TodoServiceImpl } from "./todoService";

const service = new TodoServiceImpl();

export function TodoApp() {
  const [todos, setTodos] = useState<TodoItem[]>([]);
  const [filter, setFilter] = useState<TodoFilter>("all");
  const [input, setInput] = useState("");

  const handleAddTodo = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    if (!input.trim()) return;

    const newTodo = service.addTodo(input);
    setTodos([...service.getTodos(filter), newTodo]);
    setInput("");
  };

  const handleToggleTodo = (id: number) => {
    service.toggleTodo(id);
    setTodos(service.getTodos(filter));
  };

  const handleRemoveTodo = (id: number) => {
    service.removeTodo(id);
    setTodos(service.getTodos(filter));
  };

  const displayTodos = service.getTodos(filter);

  return (
    <div>
      <form onSubmit={handleAddTodo}>
        <input
          type="text"
          value={input}
          onChange={(e) => setInput(e.currentTarget.value)}
          placeholder="Add a new todo..."
        />
        <button type="submit">Add</button>
      </form>

      <div>
        {(["all", "active", "completed"] as const).map((f) => (
          <button
            key={f}
            onClick={() => setFilter(f)}
            style={{ fontWeight: filter === f ? "bold" : "normal" }}
          >
            {f}
          </button>
        ))}
      </div>

      <ul>
        {displayTodos.map((todo) => (
          <li key={todo.id}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => handleToggleTodo(todo.id)}
            />
            <span style={{ textDecoration: todo.completed ? "line-through" : "none" }}>
              {todo.text}
            </span>
            <button onClick={() => handleRemoveTodo(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

### Project 2: Shopping Cart with Generics

```typescript
// types.ts
export interface CartItem<T> {
  id: number;
  product: T;
  quantity: number;
  addedAt: Date;
}

export interface Product {
  id: number;
  name: string;
  price: number;
}

// cart.ts
export class ShoppingCart<T extends Product> {
  private items: CartItem<T>[] = [];

  addItem(product: T, quantity: number = 1): void {
    const existingItem = this.items.find(item => item.product.id === product.id);
    if (existingItem) {
      existingItem.quantity += quantity;
    } else {
      this.items.push({
        id: Math.random(),
        product,
        quantity,
        addedAt: new Date()
      });
    }
  }

  removeItem(productId: number): void {
    this.items = this.items.filter(item => item.product.id !== productId);
  }

  getTotal(): number {
    return this.items.reduce(
      (sum, item) => sum + item.product.price * item.quantity,
      0
    );
  }

  getItems(): CartItem<T>[] {
    return this.items;
  }

  clear(): void {
    this.items = [];
  }
}

// Usage
interface Book extends Product {
  author: string;
  pages: number;
}

const bookCart = new ShoppingCart<Book>();
bookCart.addItem({
  id: 1,
  name: "TypeScript Guide",
  price: 29.99,
  author: "Myself",
  pages: 500
}, 2);

console.log(bookCart.getTotal());  // 59.98
```

### Project 3: State Management with TypeScript

```typescript
// reducer.ts
type Action = 
  | { type: "SET_LOADING"; payload: boolean }
  | { type: "SET_DATA"; payload: User[] }
  | { type: "SET_ERROR"; payload: string | null };

interface State {
  loading: boolean;
  data: User[] | null;
  error: string | null;
}

export function reducer(state: State, action: Action): State {
  switch (action.type) {
    case "SET_LOADING":
      return { ...state, loading: action.payload };
    case "SET_DATA":
      return { ...state, data: action.payload, loading: false };
    case "SET_ERROR":
      return { ...state, error: action.payload, loading: false };
    default:
      return state;
  }
}

// useAsync.ts
import { useReducer, useEffect } from "react";

interface UseAsyncState<T> {
  loading: boolean;
  data: T | null;
  error: string | null;
}

export function useAsync<T>(
  fn: () => Promise<T>
): UseAsyncState<T> {
  const [state, dispatch] = useReducer(reducer, {
    loading: true,
    data: null,
    error: null
  });

  useEffect(() => {
    let mounted = true;

    (async () => {
      try {
        dispatch({ type: "SET_LOADING", payload: true });
        const result = await fn();
        if (mounted) {
          dispatch({ type: "SET_DATA", payload: result });
        }
      } catch (error) {
        if (mounted) {
          dispatch({
            type: "SET_ERROR",
            payload: error instanceof Error ? error.message : "Unknown error"
          });
        }
      }
    })();

    return () => {
      mounted = false;
    };
  }, [fn]);

  return state as UseAsyncState<T>;
}

// Component usage
interface User {
  id: number;
  name: string;
}

export function UserList() {
  const { loading, data: users, error } = useAsync<User[]>(
    () => fetch("/api/users").then(r => r.json())
  );

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <ul>
      {users?.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

---

## Common Mistakes and How to Avoid Them

### Mistake 1: Too Many Type Assertions

```typescript
// ✗ Avoid excessive as
const user = data as unknown as User;

// ✓ Better: design types correctly from the start
const user: User = data;  // Will error if data isn't compatible
```

### Mistake 2: Overusing `any`

```typescript
// ✗ No type safety
function process(data: any) {
  return data.toUpperCase();
}

// ✓ Use union types or generics
function process(data: string | undefined) {
  return data?.toUpperCase();
}
```

### Mistake 3: Forgetting Null/Undefined Checks

```typescript
// ✗ Crashes if user is null
function getName(user: User): string {
  return user.name;  // Type says User is required
}

// ✓ Handle optional values
function getName(user: User | null): string {
  return user?.name ?? "Unknown";
}
```

### Mistake 4: Overly Complex Types

```typescript
// ✗ Too complex
type Crazy = Partial<Pick<Omit<Record<"a" | "b" | "c", string | number>, "b">, "a">>;

// ✓ Simpler and clearer
interface Simple {
  a?: string | number;
}
```

### Mistake 5: Not Using Const Generics

```typescript
// ✗ Loses specificity
function get<T extends any[]>(arr: T): T[0] {
  return arr[0];
}

const result = get([1, "hi", true]);  // Type: any

// ✓ Use const assertions or as const
function get<T extends readonly any[]>(arr: T): T[0] {
  return arr[0];
}

const result = get([1, "hi", true] as const);  // Type: 1
```

---

## Learning Resources and Next Steps

### Recommended Resources
- TypeScript Official Handbook: https://www.typescriptlang.org/docs/
- TypeScript Playground: https://www.typescriptlang.org/play
- Total TypeScript: https://www.totaltypescript.com/

### Practice Exercises
1. Convert one of your JavaScript projects to TypeScript
2. Create strict types for your API responses
3. Build a type-safe form library
4. Implement a state management system from scratch
5. Create reusable generic components

### Key Takeaways for Production Code
- Always enable strict mode in tsconfig.json
- Use interfaces/types to document your code
- Leverage generics for reusable, type-safe code
- Create discriminated unions for complex states
- Use utility types to keep types DRY
- Prioritize readability over cleverness
- Test your type assertions
- Use TypeScript's inference when appropriate, but be explicit when it matters

---

## Cheat Sheet

### Common Type Patterns

```typescript
// API Response
interface ApiResponse<T> {
  success: boolean;
  data?: T;
  error?: string;
}

// Component Props
interface ComponentProps {
  children?: ReactNode;
  className?: string;
}

// State
type State = 
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: unknown }
  | { status: "error"; error: string };

// Event Handler
type ClickHandler = (event: React.MouseEvent<HTMLElement>) => void;

// Async Function
type AsyncFunction<T> = () => Promise<T>;

// Record/Map
type UserMap = Record<string, User>;

// Tuple
type Coordinates = [number, number];

// Required/Optional
type Full = Required<Partial<User>>;
type Partial_User = Partial<User>;
```

This comprehensive guide should take you from zero to production-ready TypeScript. Start with the basics, build the projects, and gradually incorporate more advanced patterns into your code.

---

# React & Next.js Production Patterns with TypeScript

## MODULE 1: REACT FUNDAMENTALS WITH TYPESCRIPT

### 1.1 Understanding React Components

React is a library for building user interfaces. It breaks UIs into small, reusable components.

**Key concepts:**
- **Components**: Reusable pieces of UI
- **Props**: Data passed to components
- **State**: Data that changes over time
- **Rendering**: Converting component data to HTML
- **Re-rendering**: When data changes, UI updates automatically

### 1.2 Functional Components (Modern Way)

```typescript
import React from 'react';

// Simple component - no props
function Welcome() {
  return <h1>Hello, World!</h1>;
}

// Component with typed props
interface GreetingProps {
  name: string;
  age: number;
}

function Greeting({ name, age }: GreetingProps) {
  return <p>{name} is {age} years old</p>;
}

// Usage
export default function App() {
  return (
    <div>
      <Welcome />
      <Greeting name="Alice" age={25} />
    </div>
  );
}
```

### 1.3 JSX - Writing HTML in JavaScript

JSX looks like HTML but it's actually JavaScript:

```typescript
// This JSX...
const element = <h1 className="title">Hello</h1>;

// ...gets compiled to this JavaScript
const element = React.createElement('h1', { className: 'title' }, 'Hello');

// You can use expressions in JSX
const name = "Alice";
const element = <h1>Hello, {name}!</h1>;

// Conditional rendering
const element = <div>{isLoggedIn ? <Dashboard /> : <LoginForm />}</div>;

// Lists
const items = ["apple", "banana", "orange"];
const list = (
  <ul>
    {items.map((item, index) => (
      <li key={index}>{item}</li>
    ))}
  </ul>
);

// Always add key prop when rendering lists!
```

---

## MODULE 2: STATE MANAGEMENT

### 2.1 useState Hook - Simple State

```typescript
import { useState } from 'react';

interface Counter {
  count: number;
}

function Counter() {
  // useState returns [current value, function to update it]
  const [count, setCount] = useState<number>(0);

  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </div>
  );
}
```

**Best practices:**
- Initialize state with correct type: `useState<number>(0)`
- Create separate state for each value (don't combine into one object)
- Update state immutably: `setCount(count + 1)` not `count = count + 1`

### 2.2 useState with Objects

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  isAdmin: boolean;
}

function UserProfile() {
  const [user, setUser] = useState<User | null>(null);

  // Update single field
  const updateName = (newName: string) => {
    setUser(prevUser => {
      if (!prevUser) return null;
      return { ...prevUser, name: newName };
    });
  };

  // For complex updates, use previous state function
  const toggleAdmin = () => {
    setUser(prevUser => {
      if (!prevUser) return null;
      return { ...prevUser, isAdmin: !prevUser.isAdmin };
    });
  };

  if (!user) return <p>No user loaded</p>;

  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
      <p>Admin: {user.isAdmin ? "Yes" : "No"}</p>
      <button onClick={() => updateName("Bob")}>Change Name</button>
    </div>
  );
}
```

### 2.3 useReducer - Complex State

When you have multiple related state updates, use `useReducer`:

```typescript
interface AppState {
  user: User | null;
  posts: Post[];
  loading: boolean;
  error: Error | null;
}

type AppAction =
  | { type: 'SET_USER'; payload: User }
  | { type: 'SET_POSTS'; payload: Post[] }
  | { type: 'SET_LOADING'; payload: boolean }
  | { type: 'SET_ERROR'; payload: Error }
  | { type: 'CLEAR_ERROR' };

const initialState: AppState = {
  user: null,
  posts: [],
  loading: false,
  error: null
};

function appReducer(state: AppState, action: AppAction): AppState {
  switch (action.type) {
    case 'SET_USER':
      return { ...state, user: action.payload };
    case 'SET_POSTS':
      return { ...state, posts: action.payload };
    case 'SET_LOADING':
      return { ...state, loading: action.payload };
    case 'SET_ERROR':
      return { ...state, error: action.payload };
    case 'CLEAR_ERROR':
      return { ...state, error: null };
    default:
      return state;
  }
}

function App() {
  const [state, dispatch] = useReducer(appReducer, initialState);

  const loadUser = async () => {
    dispatch({ type: 'SET_LOADING', payload: true });
    try {
      const response = await fetch('/api/user');
      const user = await response.json();
      dispatch({ type: 'SET_USER', payload: user });
    } catch (error) {
      dispatch({ type: 'SET_ERROR', payload: error as Error });
    }
  };

  return (
    <div>
      {state.loading && <p>Loading...</p>}
      {state.error && <p style={{ color: 'red' }}>Error: {state.error.message}</p>}
      {state.user && <p>Welcome, {state.user.name}</p>}
    </div>
  );
}
```

---

## MODULE 3: SIDE EFFECTS WITH useEffect

### 3.1 Basic useEffect

```typescript
import { useEffect, useState } from 'react';

function UserProfile({ userId }: { userId: number }) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);

  // useEffect runs after component renders
  useEffect(() => {
    setLoading(true);
    
    fetch(`/api/users/${userId}`)
      .then(res => res.json())
      .then(data => {
        setUser(data);
        setLoading(false);
      })
      .catch(error => console.error(error));
  }, [userId]); // Dependency array - re-run when userId changes

  if (loading) return <p>Loading...</p>;
  if (!user) return <p>User not found</p>;

  return <h1>{user.name}</h1>;
}
```

### 3.2 Cleanup Functions

```typescript
function Timer() {
  const [time, setTime] = useState(0);

  useEffect(() => {
    // Set up
    const interval = setInterval(() => {
      setTime(t => t + 1);
    }, 1000);

    // Cleanup - called when component unmounts or effect re-runs
    return () => {
      clearInterval(interval);
    };
  }, []); // Empty dependency array = run once on mount

  return <p>Time: {time}s</p>;
}
```

### 3.3 Multiple Effects

```typescript
function Dashboard() {
  const [user, setUser] = useState<User | null>(null);
  const [posts, setPosts] = useState<Post[]>([]);

  // Effect 1: Load user
  useEffect(() => {
    fetch('/api/user')
      .then(r => r.json())
      .then(setUser);
  }, []);

  // Effect 2: Load posts (runs after user is set)
  useEffect(() => {
    if (!user) return; // Don't run if user isn't loaded

    fetch(`/api/users/${user.id}/posts`)
      .then(r => r.json())
      .then(setPosts);
  }, [user?.id]); // Re-run when user.id changes

  return (
    <div>
      <h1>{user?.name}</h1>
      <p>{posts.length} posts</p>
    </div>
  );
}
```

---

## MODULE 4: DATA FETCHING PATTERNS

### 4.1 Custom Hook for Fetching

```typescript
interface UseFetchOptions {
  method?: 'GET' | 'POST' | 'PUT' | 'DELETE';
  headers?: Record<string, string>;
  body?: unknown;
}

interface UseFetchResult<T> {
  data: T | null;
  loading: boolean;
  error: Error | null;
}

function useFetch<T>(
  url: string,
  options?: UseFetchOptions
): UseFetchResult<T> {
  const [state, setState] = useState<UseFetchResult<T>>({
    data: null,
    loading: true,
    error: null
  });

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch(url, {
          method: options?.method || 'GET',
          headers: options?.headers,
          body: options?.body ? JSON.stringify(options.body) : undefined
        });

        if (!response.ok) {
          throw new Error(`HTTP Error: ${response.status}`);
        }

        const data: T = await response.json();
        setState({ data, loading: false, error: null });
      } catch (error) {
        setState({
          data: null,
          loading: false,
          error: error instanceof Error ? error : new Error('Unknown error')
        });
      }
    };

    fetchData();
  }, [url]);

  return state;
}

// Usage
function UserList() {
  const { data: users, loading, error } = useFetch<User[]>('/api/users');

  if (loading) return <p>Loading users...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return (
    <ul>
      {users?.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

### 4.2 Request State Pattern

```typescript
interface RequestState<T> {
  status: 'idle' | 'loading' | 'success' | 'error';
  data: T | null;
  error: Error | null;
}

type RequestAction<T> =
  | { type: 'START' }
  | { type: 'SUCCESS'; payload: T }
  | { type: 'ERROR'; payload: Error };

function requestReducer<T>(
  state: RequestState<T>,
  action: RequestAction<T>
): RequestState<T> {
  switch (action.type) {
    case 'START':
      return { status: 'loading', data: null, error: null };
    case 'SUCCESS':
      return { status: 'success', data: action.payload, error: null };
    case 'ERROR':
      return { status: 'error', data: null, error: action.payload };
  }
}

function UserDetail({ userId }: { userId: number }) {
  const [state, dispatch] = useReducer(requestReducer, {
    status: 'idle',
    data: null,
    error: null
  });

  useEffect(() => {
    dispatch({ type: 'START' });

    fetch(`/api/users/${userId}`)
      .then(r => r.json())
      .then(user => dispatch({ type: 'SUCCESS', payload: user }))
      .catch(error => dispatch({ type: 'ERROR', payload: error }));
  }, [userId]);

  switch (state.status) {
    case 'idle':
      return <p>Not started</p>;
    case 'loading':
      return <p>Loading...</p>;
    case 'success':
      return <h1>{state.data?.name}</h1>;
    case 'error':
      return <p style={{ color: 'red' }}>Error: {state.error?.message}</p>;
  }
}
```

---

## MODULE 5: REUSABLE CUSTOM HOOKS

### 5.1 useLocalStorage Hook

```typescript
function useLocalStorage<T>(
  key: string,
  initialValue: T
): [T, (value: T) => void] {
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(error);
      return initialValue;
    }
  });

  const setValue = (value: T) => {
    try {
      setStoredValue(value);
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error(error);
    }
  };

  return [storedValue, setValue];
}

// Usage
function App() {
  const [theme, setTheme] = useLocalStorage<'light' | 'dark'>('theme', 'light');

  return (
    <div style={{ background: theme === 'light' ? 'white' : 'black' }}>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </div>
  );
}
```

### 5.2 useDebounce Hook

```typescript
function useDebounce<T>(value: T, delay: number): T {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => clearTimeout(handler);
  }, [value, delay]);

  return debouncedValue;
}

// Usage - search with API calls
function SearchUsers() {
  const [searchTerm, setSearchTerm] = useState('');
  const debouncedSearchTerm = useDebounce(searchTerm, 500);
  const { data: results } = useFetch<User[]>(
    `/api/users/search?q=${debouncedSearchTerm}`
  );

  return (
    <div>
      <input
        value={searchTerm}
        onChange={e => setSearchTerm(e.target.value)}
        placeholder="Search users..."
      />
      <ul>
        {results?.map(user => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

### 5.3 useAsync Hook

```typescript
interface UseAsyncOptions {
  immediate?: boolean;
}

function useAsync<T>(
  asyncFunction: () => Promise<T>,
  options?: UseAsyncOptions
) {
  const [state, setState] = useState<{
    data: T | null;
    loading: boolean;
    error: Error | null;
  }>({
    data: null,
    loading: options?.immediate !== false,
    error: null
  });

  const execute = useCallback(async () => {
    setState({ data: null, loading: true, error: null });
    try {
      const response = await asyncFunction();
      setState({ data: response, loading: false, error: null });
    } catch (error) {
      setState({
        data: null,
        loading: false,
        error: error instanceof Error ? error : new Error('Unknown error')
      });
    }
  }, [asyncFunction]);

  useEffect(() => {
    if (options?.immediate !== false) {
      execute();
    }
  }, [execute, options?.immediate]);

  return { ...state, execute };
}

// Usage
function UserProfile() {
  const fetchUser = useCallback(() => fetch('/api/user').then(r => r.json()), []);
  const { data: user, loading, error, execute: refetch } = useAsync(fetchUser);

  return (
    <div>
      {loading && <p>Loading...</p>}
      {error && <p>Error: {error.message}</p>}
      {user && (
        <>
          <h1>{user.name}</h1>
          <button onClick={refetch}>Refresh</button>
        </>
      )}
    </div>
  );
}
```

---

## MODULE 6: CONTEXT API FOR STATE MANAGEMENT

### 6.1 Creating and Using Context

```typescript
import React, { createContext, useContext, useState } from 'react';

interface AuthContextType {
  user: User | null;
  isLoggedIn: boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export function AuthProvider({ children }: { children: React.ReactNode }) {
  const [user, setUser] = useState<User | null>(null);

  const login = async (email: string, password: string) => {
    const response = await fetch('/api/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password })
    });
    const userData = await response.json();
    setUser(userData);
  };

  const logout = () => {
    setUser(null);
  };

  const value: AuthContextType = {
    user,
    isLoggedIn: !!user,
    login,
    logout
  };

  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  const context = useContext(AuthContext);
  if (context === undefined) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
}
```

### 6.2 Using Context in Components

```typescript
function Dashboard() {
  const { user, logout } = useAuth();

  if (!user) {
    return <p>Not logged in</p>;
  }

  return (
    <div>
      <h1>Welcome, {user.name}</h1>
      <p>Email: {user.email}</p>
      <button onClick={logout}>Logout</button>
    </div>
  );
}

function App() {
  return (
    <AuthProvider>
      <Dashboard />
    </AuthProvider>
  );
}
```

---

## MODULE 7: NEXT.JS SPECIFIC PATTERNS

### 7.1 App Router Basics

```typescript
// app/page.tsx - Home page
export default function Home() {
  return <h1>Welcome to Next.js</h1>;
}

// app/users/page.tsx - Users list page
export default function Users() {
  return <h1>Users</h1>;
}

// app/users/[id]/page.tsx - Dynamic route
interface PageProps {
  params: {
    id: string;
  };
}

export default function UserDetail({ params }: PageProps) {
  return <h1>User {params.id}</h1>;
}
```

### 7.2 Server Components vs Client Components

```typescript
// app/components/ServerComponent.tsx (default)
// Runs on server - good for fetching data
export default function ServerComponent() {
  // Can access databases, APIs, secrets directly
  return <h1>Server Component</h1>;
}

// app/components/ClientComponent.tsx
'use client' // Mark as client component

import { useState } from 'react';

export default function ClientComponent() {
  // Can use hooks, interactivity
  const [count, setCount] = useState(0);
  
  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}
```

### 7.3 Server-Side Data Fetching

```typescript
// app/users/page.tsx
import { User } from '@/types/user';

async function getUsers(): Promise<User[]> {
  const response = await fetch('https://api.example.com/users', {
    cache: 'revalidate' // Revalidate every 60 seconds
  });
  return response.json();
}

export default async function UsersPage() {
  const users = await getUsers();

  return (
    <div>
      <h1>Users</h1>
      <ul>
        {users.map(user => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

### 7.4 Client-Side Data Fetching

```typescript
'use client'

import { useEffect, useState } from 'react';

interface Post {
  id: number;
  title: string;
}

export default function PostsPage() {
  const [posts, setPosts] = useState<Post[]>([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch('/api/posts')
      .then(r => r.json())
      .then(data => {
        setPosts(data);
        setLoading(false);
      });
  }, []);

  if (loading) return <p>Loading...</p>;

  return (
    <ul>
      {posts.map(post => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

### 7.5 Route Handlers (API Routes)

```typescript
// app/api/users/route.ts
import { NextRequest, NextResponse } from 'next/server';

export async function GET(request: NextRequest) {
  const users = [
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' }
  ];
  
  return NextResponse.json(users);
}

export async function POST(request: NextRequest) {
  const body = await request.json();
  
  // Save user to database
  const newUser = { id: 3, ...body };
  
  return NextResponse.json(newUser, { status: 201 });
}

// app/api/users/[id]/route.ts
export async function GET(
  request: NextRequest,
  { params }: { params: { id: string } }
) {
  const user = { id: params.id, name: 'Alice' };
  return NextResponse.json(user);
}

export async function PUT(
  request: NextRequest,
  { params }: { params: { id: string } }
) {
  const body = await request.json();
  const updatedUser = { id: params.id, ...body };
  
  return NextResponse.json(updatedUser);
}
```

---

## MODULE 8: BEST PRACTICES & PERFORMANCE

### 8.1 Component Structure

```typescript
// Good structure - organized and maintainable
interface UserCardProps {
  userId: number;
}

export function UserCard({ userId }: UserCardProps) {
  const { data: user, loading } = useFetch(`/api/users/${userId}`);

  if (loading) return <div>Loading...</div>;
  if (!user) return <div>User not found</div>;

  return (
    <article>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </article>
  );
}

export default UserCard;
```

### 8.2 Performance Optimization

```typescript
'use client'

import { useMemo, useCallback } from 'react';

interface ListProps {
  items: Item[];
  onSelect: (item: Item) => void;
}

function List({ items, onSelect }: ListProps) {
  // Memoize expensive calculations
  const sortedItems = useMemo(() => {
    return [...items].sort((a, b) => a.name.localeCompare(b.name));
  }, [items]);

  // Memoize callbacks to prevent re-renders
  const handleSelect = useCallback((item: Item) => {
    onSelect(item);
  }, [onSelect]);

  return (
    <ul>
      {sortedItems.map(item => (
        <li key={item.id} onClick={() => handleSelect(item)}>
          {item.name}
        </li>
      ))}
    </ul>
  );
}
```

### 8.3 Error Boundaries

```typescript
interface ErrorBoundaryProps {
  children: React.ReactNode;
}

interface ErrorBoundaryState {
  hasError: boolean;
  error: Error | null;
}

export class ErrorBoundary extends React.Component<
  ErrorBoundaryProps,
  ErrorBoundaryState
> {
  constructor(props: ErrorBoundaryProps) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error: Error) {
    return { hasError: true, error };
  }

  render() {
    if (this.state.hasError) {
      return (
        <div style={{ padding: '20px', color: 'red' }}>
          <h2>Something went wrong</h2>
          <p>{this.state.error?.message}</p>
        </div>
      );
    }

    return this.props.children;
  }
}

// Usage
function App() {
  return (
    <ErrorBoundary>
      <Dashboard />
    </ErrorBoundary>
  );
}
```

### 8.4 Type-Safe API Calls

```typescript
// types/api.ts
export interface ApiResponse<T> {
  success: boolean;
  data?: T;
  error?: string;
}

// services/api.ts
const API_BASE_URL = process.env.REACT_APP_API_URL;

export async function apiCall<T>(
  endpoint: string,
  options?: RequestInit
): Promise<T> {
  const response = await fetch(`${API_BASE_URL}${endpoint}`, {
    headers: { 'Content-Type': 'application/json' },
    ...options
  });

  if (!response.ok) {
    throw new Error(`API Error: ${response.status}`);
  }

  const data: ApiResponse<T> = await response.json();

  if (!data.success) {
    throw new Error(data.error || 'Unknown error');
  }

  if (!data.data) {
    throw new Error('No data returned');
  }

  return data.data;
}

// Usage
async function loadUsers() {
  const users = await apiCall<User[]>('/users');
  return users;
}
```

---
