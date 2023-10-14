Rust Variables:
The way to define a variables is with the keyword let, like this:
let bunnies = 2;

When the value can be easily infered, Rust will do it automaticaly, I thinkthis is similar to the var keyword in c# but here you can correct me if I'm wrong GPT.
The way to add the type annotation is by adding this code:

```rust
let bunnies = 2;
```
let bunnies**: i32** = 2; //This way, we're telling the compiler to treat this as a 32 byte integer, I think

There's also a way to declare and assign multiple variables in the same line:

```rust
let (bunnies, carrots) = (8, 50)
```

Here you can add an example of using explicit type annotation GPT.

Variables are inmmutable by default in Rust, which means that unless we make them mutable we cannot change their values in the code, thus, failing to run when trying.
Here please add an explanation on to why they're unmutable by default, pros and cons, (safety, concurrency, speed, etc).

We can make a mutable variable with this code
```rust
let mut bunnies = 32;
bunnies = 2; //This is ok, we can change it many times without errors.
```

In Rust we also have constants, these are of couse, inmmutable.
Here GPT, please add a bunch of information about rust constants, like why and when we should use them, pros and cons against variables, etc.

```rust
const WARP_FACTOR: f64 = 9.9; //The value must be a constant expression!!!
//constants requires the type annotation and are snake-case by convention.
```