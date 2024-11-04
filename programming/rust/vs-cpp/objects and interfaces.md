# Rust vs C++: Objects &amp; Interfaces

## Inheritance

C++ has inheritance.
Rust does not.
However, you can more-or-less emulate inheritance with a `base` field, and perhaps some <code>impl [Deref](https://doc.rust-lang.org/core/ops/trait.Deref.html)\[[Mut](https://doc.rust-lang.org/core/ops/trait.DerefMut.html)\] for Derived</code>s.

### C++
```cpp
#include <cstdint>
#include <string>

struct Base {
    uint32_t        a;
    std::u8string   b;
};

struct Derived : Base {
    uint32_t        c;
};

int main() {
    Derived d {};
    d.a = 1;
    d.b = u8"Hello, world!";
    d.c = 3;
}
```

### Rust

```rust
#[derive(Default)] struct Base {
    pub a:  u32,
    pub b:  String,
}

#[derive(Default)] struct Derived {
    pub base:   Base,
    pub c:      u32,
}

fn main() {
    let mut d = Derived::default();
    d.base.a = 1; // can be simplified to `d.a = 1;` with DerefMut
    d.base.b = "Hello, world!".into();
    d.c = 3;
}

// N.B. I typically wouldn't bother with this.
// I also wouldn't call it idiomatic Rust.
// I *might* use it as a temporary measure when rewriting C++ to Rust.

impl core::ops::Deref for Derived {
    type Target = Base;
    fn deref(&self) -> &Base {
        &self.base
    }
}

impl core::ops::DerefMut for Derived {
    fn deref_mut(&mut self) -> &mut Base {
        &mut self.base
    }
}
```

## Interfaces &amp; Dynamic Dispatch

Despite the lack of inheritance, Rust *can* do interfaces, dynamic dispatch, and type erasure.
Note that `trait`s don't get to define data, and the memory layout is significantly different.

### C++

```cpp
#include <iostream>
#include <memory>

struct Animal {
    virtual ~Animal() {} // avoid leaks/UB if you `delete` a derived class through `Animal*`
    virtual void speak() const = 0;
    virtual void sleep() const { std::cout << "Sleeping.\n"; }
};

struct Cat : Animal {
    uint32_t remaining_lives = 9;
    void speak() const override { std::cout << "Meow.\n"; }
    void sleep() const { std::cout << "Sleeping in a sunspot.\n"; }
};

struct Dog : Animal {
    uint32_t age = 0;
    void speak() const override { std::cout << "Woof.\n"; }
};

void play_with(const Animal& animal) {
    std::cout << "Speak, boy!\n";
    animal.speak(); // dynamic dispatch
}

int main() {
    std::unique_ptr<Animal> animal = std::make_unique<Cat>(); // type erasure
    play_with(*animal);
}
```

### Rust

```rust
trait Animal {
    fn speak(&self);
    fn sleep(&self) { println!("Sleeping.") }
}

struct Cat {
    pub remaining_lives: u32,
}
impl Default for Cat {
    fn default() -> Self { Self { remaining_lives: 9 } }
}
impl Animal for Cat {
    fn speak(&self) { println!("Meow.") }
    fn sleep(&self) { println!("Sleepingin a sunspot.") }
}

struct Dog {
    pub age: u32,
}
impl Default for Dog {
    fn default() -> Self { Self { age: 0 } }
}
impl Animal for Dog {
    fn speak(&self) { println!("Woof.") }
}

fn play_with(animal: &dyn Animal) {
    println!("Speak, boy!");
    animal.speak(); // dynamic dispatch
}

fn main() {
    let animal : Box<dyn Animal> = Box::new(Cat{}); // type erasure
    play_with(&*animal);
}
```

### Possible C++ Memory Layout

C++ puts a pointer to the vtable inside `Cat` and `Dog`.
For a registerless, 100% stack based machine, an example possible memory layout might be:

```text
0x55550000  The stack:
    ...
0x55550000      main:
0x55550004          animal  = 0xEEEE0100 = &Cat#1
    ...
0x55550010      play_with:
0x55550014          animal  = 0xEEEE0100 = &Cat#1
    ...
0x55550020      Cat::speak:
0x55550024          this    = 0xEEEE0100 = &Cat#1



0xEEEE0000  Heap allocations:
    ...
0xEEEE0100      Cat#1:
0xEEEE0100          Cat::__vfptr            = 0xDDDD0100 = &Cat::__vtable
0xEEEE0104          Cat::remaining_lives    = 0x00000009 = 9



0xCCCC0000  Code (read-only, part of the Executable):
    ...
0xCCCC0100      Animal::~Animal { ... }
0xCCCC0200      Animal::sleep   { ... }
    ...
0xCCCC0300      Cat::~Cat       { ... }
0xCCCC0400      Cat::speak      { ... }
0xCCCC0500      Cat::sleep      { ... }
    ...
0xCCCC0600      Dog::~Dog       { ... }
0xCCCC0700      Dog::speak      { ... }



0xDDDD0000  Data (constant/read-only, part of the executable):
    ...
0xDDDD0100      Cat::__vtable:
0xDDDD0100          &Cat::~Cat      = 0xCCCC0300
0xDDDD0104          &Cat::speak     = 0xCCCC0400
0xDDDD0108          &Cat::sleep     = 0xCCCC0500
    ...
0xDDDD0200      Dog::__vtable:
0xDDDD0200          &Dog::~Dog      = 0xCCCC0600
0xDDDD0204          &Dog::speak     = 0xCCCC0700
0xDDDD0208          &Animal::sleep  = 0xCCCC0500
```

### Possible Rust Memory Layout

Rust does **not** put a pointer to the vtable inside `Cat` nor `Dog`.
Instead, `&dyn Animal` and `Box<dyn Animal>` are "fat pointers": twice the width of a normal pointer, containing *two* addresses under the hood:
one is a pointer to the animal's data, just like C++.  The other other is a pointer to the vtable.
For a registerless, 100% stack based machine, an example possible memory layout might be:

```text
0x55550000  The stack:
    ...
0x55550000      main:
0x55550004          animal.data     = 0xEEEE0100 = &Cat#1
0x55550008          animal.trait    = 0xDDDD0100 = impl Animal for Cat
    ...
0x55550010      play_with:
0x55550014          animal.data     = 0xEEEE0100 = &Cat#1
0x55550018          animal.trait    = 0xDDDD0100 = impl Animal for Cat
    ...
0x55550020      Cat::speak:
0x55550024          self            = 0xEEEE0100 = &Cat#1



0xEEEE0000  Heap allocations:
    ...
0xEEEE0100      Cat#1:
0xEEEE0100          Cat::remaining_lives    = 0x00000009 = 9



0xCCCC0000  Code (read-only, part of the Executable):
    ...
0xCCCC0100      <Animal as Drop>::drop  { ... }
0xCCCC0200      Animal::sleep           { ... }
    ...
0xCCCC0300      <Cat as Drop  >::drop   { ... }
0xCCCC0400      <Cat as Animal>::speak  { ... }
0xCCCC0500      <Cat as Animal>::sleep  { ... }
    ...
0xCCCC0600      <Dog as Drop  >::drop   { ... }
0xCCCC0700      <Dog as Animal>::speak  { ... }



0xDDDD0000  Data (constant/read-only, part of the executable):
    ...
0xDDDD0100      impl Animal for Cat
0xDDDD0100          &Drop::drop     =   ???
0xDDDD0104          &Cat::speak     = 0xCCCC0400
0xDDDD0108          &Cat::sleep     = 0xCCCC0500
    ...
0xDDDD0200      impl Animal for Dog
0xDDDD0200          &Drop::drop     =   ???
0xDDDD0204          &Dog::speak     = 0xCCCC0700
0xDDDD0208          &Animal::sleep  = 0xCCCC0500
```
