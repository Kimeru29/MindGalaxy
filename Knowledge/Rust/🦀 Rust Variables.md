# 🦀 Rust Variables Basics

- **Tags**: #Rust #Programming #Variables
- **Related**: [[Rust Functions]], [[Rust Data Types]]

---

## 📝 Table of Contents

1. [Basic Variable Declaration](https://chat.openai.com/c/7cfb0893-62aa-4772-ae94-342687678b79#1%EF%B8%8F%E2%83%A3-Basic-Variable-Declaration)
2. [Type Inference & Annotation](https://chat.openai.com/c/7cfb0893-62aa-4772-ae94-342687678b79#2%EF%B8%8F%E2%83%A3-Type-Inference--Annotation)
3. [Multiple Variables](https://chat.openai.com/c/7cfb0893-62aa-4772-ae94-342687678b79#3%EF%B8%8F%E2%83%A3-Multiple-Variables)
4. [Immutability](https://chat.openai.com/c/7cfb0893-62aa-4772-ae94-342687678b79#4%EF%B8%8F%E2%83%A3-Immutability)
5. [Mutable Variables](https://chat.openai.com/c/7cfb0893-62aa-4772-ae94-342687678b79#5%EF%B8%8F%E2%83%A3-Mutable-Variables)
6. [Constants](https://chat.openai.com/c/7cfb0893-62aa-4772-ae94-342687678b79#6%EF%B8%8F%E2%83%A3-Constants)

---

## 1️⃣ Basic Variable Declaration

In Rust, variables are defined using the `let` keyword.

rustCopy code

`let bunnies = 2;`

---

## 2️⃣ Type Inference & Annotation

### 📚 Type Inference

Rust can automatically infer the type of the variable based on its value, similar to `var` in C#. However, Rust's type inference is more stringent.

rustCopy code

`let bunnies = 2; // Inferred as i32`

### 🏷️ Type Annotation

You can explicitly define the type of a variable like this:

rustCopy code

`let bunnies: i32 = 2;`

---

## 3️⃣ Multiple Variables

You can declare and assign multiple variables in a single line:

rustCopy code

`let (bunnies, carrots) = (8, 50);`

With type annotation:

rustCopy code

`let (bunnies: i32, carrots: i32) = (8, 50);`

---

## 4️⃣ Immutability

### 🛡️ Why Immutable by Default?

1. **Safety**: Prevents accidental data modification.
2. **Concurrency**: Easier to manage in multi-threaded environments.
3. **Performance**: The compiler can make optimizations.

**Example**

If you try to modify an immutable variable, the compiler will throw an error.

rustCopy code

`let bunnies = 2; bunnies = 4; // ❌ Error`

---

## 5️⃣ Mutable Variables

Use the `mut` keyword to declare a variable as mutable.

rustCopy code

`let mut bunnies = 32; bunnies = 2; // ✅ No error`

---

## 6️⃣ Constants

### 🔒 Characteristics

1. **Immutable**: Cannot be changed after declaration.
2. **Type Annotation Required**: Must specify type.
3. **Constant Expression**: The value must be constant.

**When to Use Constants**

1. **Global Settings**: Such as configurations that should not be modified.
2. **Mathematical Values**: Like π, where the value is fixed.

**Example**

rustCopy code

`const WARP_FACTOR: f64 = 9.9;`