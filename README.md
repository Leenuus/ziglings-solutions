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

### Assignment Semantic

This is important to figure out __how assignment works__ when learning a new programming language.

In Zig, assignment works this way:

1. When assigning primitive type to a new variable, Zig allocates memory for this variable, and copies the primitive type value into this memory slot. This works like any other languages.
2. When assigning complex type like a struct, Zig still copies this value to the new variable.
3. Function Parameter works the same way, the default semantic is copy semantic. It is not the same as those dynammic languages like Python and Javascript(And a non-dynamic one, Java). When these languages receive a reference to complex type, this reference binds to the variable, you can use this __reference__ to __access and modify its fields__. But when you modify the structure or class itself, (btw, the only way to do so is to rebind it)you __lost the reference to the passed in value__.
4. In Rust, semantic gets more complicated. You can use `mut var: type`,  `mut var: &type`,  `mut var: &mut type`,   `var: &type`,   `var: &mut type`,  `var: type` to specify the incoming parameter. The `mut` comes in front of the variable name `var` indicates whether `var` is rebindable. And the following annotions indicate the type of the parameter. `&mut type` means mutable reference; you use this type to modify the value outside of the function, invoking side effects. `&type` means immutable reference; you can only read the variable, not allowed to change it. `type` is a way to consume the value passed in, which indicates you don't need this value anymore after executing the function, so Rust compiler could safely release the memory it takes.
5. Implicit assignment. Code like `var a = struct A{}; var b = [a, a];` __works differently__ in different languages too. But in fact, it is just assignment. So for languages like C, Zig and Cpp, the array `b` just gets two copy of `a`, consuming it own space.

### Pointers

Are all of these pointer types starting to get confusing?

    FREE ZIG POINTER CHEATSHEET! (Using u8 as the example type.)
  +---------------+----------------------------------------------+
  |  u8           |  one u8                                      |
  |  *u8          |  pointer to one u8                           |
  |  [2]u8        |  two u8s                                     |
  |  [*]u8        |  pointer to unknown number of u8s            |
  |  [*]const u8  |  pointer to unknown number of immutable u8s  |
  |  *[2]u8       |  pointer to an array of 2 u8s                |
  |  *const [2]u8 |  pointer to an immutable array of 2 u8s      |
  |  []u8         |  slice of u8s                                |
  |  []const u8   |  slice of immutable u8s                      |
  +---------------+----------------------------------------------+


### Enum and Union

Enum with wrapped value is somehow union in fact.

```Rust
enum Fruit{
  Apple(Apple),
  Grape(Grape),
}

struct Apple;
struct Grape;

```

I mean, union is a way to save memory in machine level, but __in a programmer perspctive__, it gives a way to express these things are things in the same group. And what about enum? It is a way to express that there are A, B, C, D, total 4 options in this circumstances. So when you combine two concepts, you get a way to indicate a group, also a way to specify the certain thing belonged to this group. To be short, __union creates a namespace holding the options, and enum indicates one option in this namespace__

So in Zig, there is something like

```Zig

const Insect = union(enum) {
    flowers_visited: u16,
    still_alive: bool,
};

// The same as the following
const InsectStat = enum { flowers_visited, still_alive };

const Insect = union(InsectStat) {
    flowers_visited: u16,
    still_alive: bool,
};


```