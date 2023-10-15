# 🦀 Rust Variable Shadowing

- **Tags**: #Rust #Programming #VariableShadowing
- **Related**: [[🦀 Rust Variables]], [[🦀 Rust Variable Types]]
- **Audience**: Intermediate Rust Developers

---

## 📝 Table of Contents

1. [Same Scope Shadowing](https://chat.openai.com/c/ff5727ad-ade4-491a-84c9-283bef4dbbaf#%F0%9F%8C%93-same-scope-shadowing)
2. [Cross-Type Shadowing](https://chat.openai.com/c/ff5727ad-ade4-491a-84c9-283bef4dbbaf#%F0%9F%94%84-cross-type-shadowing)

---

## 🌓 Same Scope Shadowing

### 💡 What's the Deal?

In Rust, you can shadow a variable even within the same scope. This allows you to reuse variable names.

**Example**:
```rust
let x = 5;
let x = x * 2;
let x = x + 3;
println!("x = {}", x); // Output: x = 13
```
### 🤔 When is it Useful?

When you need to transform a variable but want to keep the original name, shadowing in the same scope can be beneficial.

---

## 🔄 Cross-Type Shadowing

### 💡 Different Types, Same Name

In Rust, shadowing allows you to change the type of a variable while keeping the same name.

**Example**:
```rust
let space = "   ";
let space = space.len();
```

### 👍 Pros and 👎 Cons

**Pros**:

1. Code readability.
2. Reuse variable names.

**Cons**:

1. Might lead to confusion.
2. Type errors can be masked.
