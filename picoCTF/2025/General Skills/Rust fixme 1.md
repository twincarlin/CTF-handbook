# Rust fixme 1 — picoCTF 2025

Hints:
1. Cargo is Rust's package manager and will make your life easier. See the getting started page [here](https://doc.rust-lang.org/book/ch01-03-hello-cargo.html)
2. [println!](https://doc.rust-lang.org/std/macro.println.html)
3. Rust has some pretty great compiler error messages. Read them maybe?

I saw the copiler errors in [Online Rust Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024) but I could not add the `xor_cryptor` dependency in the `Cargo.toml`.

Decided to setup Rust and rust-analyzer in VSCode and fix the syntax, compile and run it there.

Corrected the following lines in the `main.rs`:

```
let key = String::from("CSUCKS"); // How do we end statements in Rust?
return; // How do we return in rust?
"{}", // How do we print out a variable in the println function? 
```

And the ran `cargo run` in the terminal to get the flag.
