# 🦀 Rust Variable Types

- **Tags**: #Rust #Programming #VariableTypes
- **Related**: [[🦀 Rust Variables]], [[🦀 Rust Variable Shadowing]]
- **Audience**: Beginners to Intermediate Rust Developers

---

## 📝 Table of Contents

1. [Numeric Types](https://chat.openai.com/c/ff5727ad-ade4-491a-84c9-283bef4dbbaf#%F0%9F%94%A2-numeric-types)
2. [Character and String Types](https://chat.openai.com/c/ff5727ad-ade4-491a-84c9-283bef4dbbaf#%F0%9F%85%B0%EF%B8%8F-character-and-string-types)
3. [Boolean Type](https://chat.openai.com/c/ff5727ad-ade4-491a-84c9-283bef4dbbaf#%F0%9F%85%B1%EF%B8%8F-boolean-type)
4. [Compound Types](https://chat.openai.com/c/ff5727ad-ade4-491a-84c9-283bef4dbbaf#%F0%9F%91%A5-compound-types)
5. [Optional Type](https://chat.openai.com/c/ff5727ad-ade4-491a-84c9-283bef4dbbaf#%F0%9F%93%A6-optional-type)
6. [References](https://chat.openai.com/c/ff5727ad-ade4-491a-84c9-283bef4dbbaf#%F0%9F%93%8E-references)
7. [Enums](https://chat.openai.com/c/ff5727ad-ade4-491a-84c9-283bef4dbbaf#%F0%9F%93%84-enums)
8. [Others](https://chat.openai.com/c/ff5727ad-ade4-491a-84c9-283bef4dbbaf#%F0%9F%91%BE-others)

---

## 🔢 Numeric Types

### Integers

- **i8, i16, i32, i64, i128**: Signed, with different bit lengths.
    - `let small_num: i8 = -128;`

### Unsigned Integers

- **u8, u16, u32, u64, u128**: No negatives here!
    - `let byte: u8 = 255;`

### Floating Points

- **f32, f64**: For all your decimal needs.
    - `let pi: f64 = 3.14159;`

---

## 🅰️ Character and String Types

- **char**: One character, but any Unicode character.
    - `let letter: char = 'A';`
- **String**: More than one char, strings!
    - `let word: String = String::from("hello");`

---

## 🅱️ Boolean Type

- **bool**: True or false, that's it.
    - `let is_true: bool = true;`

---

## 👥 Compound Types

### Tuples

- Can hold different types.
    - `let tuple: (i32, f64, u8) = (500, 6.4, 1);`

### Arrays

- All elements are the same type.
    - `let arr: [i32; 5] = [1, 2, 3, 4, 5];`

---

## 📦 Optional Type

- **Option<T>**: Got a value or nah?
    - `let x: Option<i32> = Some(5);`

---

## 📎 References

- **&T, &mut T**: Refer to things without taking ownership.
    - `let mut x = 5; let y = &mut x;`

---

## 📄 Enums

- Custom types with different variants.
    - `enum Direction { North, South, East, West }`

---

## 👾 Others

### Function Pointers

- **fn**: Point to a function.
    - `fn add(a: i32, b: i32) -> i32 { a + b }`

### Dynamic Dispatch

- **dyn Trait**: Multiple types in one.
    - `let x: &dyn ToString = &5;`