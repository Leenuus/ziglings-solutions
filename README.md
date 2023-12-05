# Ziglings Solution
# [For the Origin Repo, here](https://codeberg.org/ziglings/exercises/)

## Some Learning Notes

### Characteristic of Zig lang

1. A very extensive syntax, full of features from other languages.
2. `defer` and `errdefer` keyword makes life easier
3. treat errors as return value in the form of `enum`, btw, *Zig*'s `enum` is much more understandable and writable than the *Rust* one
4. A much more powerful `switch` statement
5. `unreachable` keyword optimization hints to the compiler, also as placeholder for programmer
6. A much more understandable type system, **without annoying nested type** visually. A *Rust* programmer will thank it for **no crazy type annotation**. Think about a `Result` with `Option` inside adding generic annotation.
7. As for function, it follows a convetional way same as *C* function, but the **parameter is not rebindable**, like *Rust* function parameter without a `mut` in front of it.
8. Following point 7, we can have multiple name to the same thing in memory, however, *Zig* forces **all declared variable should be used**, helping us get rid of confusion, without the bothering to persuade the *borrow checker*
9. No macro system, so there won't be extra annotations everywhere and also symbols which you can't know whether it is a macro. Maybe `comptime` code generation is a better idea.