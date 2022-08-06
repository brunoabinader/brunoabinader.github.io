---
title: "Converting C-Like Enums to String values in Rust"
date: 2014-01-28
---

> **Note**: The contents of this post are based on an API that is now deprecated. For latest guidance over Rust usage of
enums from a C/C++ perspective, I recommend reading fasterthanlime's [_Peeking inside a rust
enum_](https://fasterthanli.me/articles/peeking-inside-a-rust-enum) guide.

Suppose you have a C-like enumerator in Rust:

``` rust
enum Foo {
    SomeValue,
    OtherValue
}
```

> **Note**: C-Like enumerations in Rust have no parameters (i.e. `SomeValue(~str)`), and can have their discriminator values explicitly set to a constant value (i.e. `SomeValue = 0`). See the Rust book [section 4.13 Enums](https://static.rust-lang.org/doc/master/book/enums.html) for details.

If you want to obtain the string value (i.e. "SomeValue") given an unsigned integer, the trick is to first cast it as a `Foo` value, then use [format](https://doc.rust-lang.org/std/fmt/) to obtain its string representation. For the former step, first we need to make `Foo` implement the [CLike](http://static.rust-lang.org/doc/master/collections/enum_set/trait.CLike.html) [trait](http://static.rust-lang.org/doc/master/tutorial.html#traits):

``` rust
extern mod extra;

use std::cast;
use extra::enum_set::CLike;

#[repr(uint)]
enum Foo {
    SomeValue,
    OtherValue
}

impl CLike for Foo {
    fn to_uint(&self) -> uint {
        *self as uint
    }
    fn from_uint(v: uint) -> Foo {
        unsafe { cast::transmute(v) }
    }
}
```

The latter step is easy, we just need to format the output as string.
The simplest way is to make `Foo` derive from `ToStr`:

``` rust
extern mod extra;

use std::cast;
use extra::enum_set::CLike;
use std::to_str::ToStr;

#[repr(uint)]
#[deriving(ToStr)]
enum Foo {
    SomeValue,
    OtherValue
}

impl CLike for Foo {
    fn to_uint(&self) -> uint {
        *self as uint
    }
    fn from_uint(v: uint) -> Foo {
        unsafe { cast::transmute(v) }
    }
}

fn main() {
    let i : Foo = CLike::from_uint(1);
    println!("{}", i.to_str());
}
```

The output for this code is `SomeValue`.
