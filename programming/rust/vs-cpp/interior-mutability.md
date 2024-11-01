# Rust vs C++: Interior Mutability

See also [The Rust PL 2nd Ed.](https://doc.rust-lang.org/book/ch15-05-interior-mutability.html) on Interior Mutability.

Like in C++, the `const`ness of the outer object can - but does not always - affect access to it's members.
For example, given `const std::vector<int> & v`, you cannot modify the elements of the vector with `v[0] = 42;` - this will fail to compile.
Given a `const gsl::span<int> & av`, however, `av[0] = 42;` will compile just fine.
Rust calls this second case "interior mutability".

Another example:
In C++, given `const some_struct & s`, you cannot modify members with `s.some_field = 42;` unless `some_field` used the `mutable` keyword.
Similarly, in Rust, given `s : &SomeStruct`, you cannot modify members with `s.some_field = 42;` unless some_field was wrapped in `Cell` or another type that provides "interior mutability".

Because immutability is tied to the ability to share a value in Rust, "interior mutability" is used in some places that may be suprising to a C++ programmer.
For example, you can modify an "immutable" Rust `std::sync::atomic::AtomicUsize`, even though you wouldn't be able to modify a C++ `const std::atomic<size_t>`, because `AtomicUsize` has been given "interior mutability".
Without interior mutability, it'd be impossible to share a AtomicUsize between multiple threads while allowing write access - defeating the entire purpouse of having atomic types in the first place.

Some of the types in Rust providing interior mutability - generally, you only need one at a time:

| Rust Type | Description |
| ----------| ------------|
| <code>std::cell::[Cell](https://doc.rust-lang.org/std/cell/struct.Cell.html)&lt;T&gt;</code> | Exists specifically to give you Interior Mutability for "Free". This type works by giving you methods like `get()`, and `set(value)`, and enforcing at compile time that you never hold onto a `&T` or `&mut T` (unless you have an exclusive mutable reference to the whole `Cell` at once.) Cannot be shared between threads. `T` must be `Copy` able.
| <code>std::cell::[RefCell](https://doc.rust-lang.org/std/cell/struct.RefCell.html)&lt;T&gt;</code> | Exists specifically to give you Interior Mutability. Unlike `Cell<T>`, this allows you to get a `&T` or `&mut T` from an immutable instance of `RefCell<T>`, and instead enforces at run time that you'll never have a `&mut T` and any other references at the same time. Methods like `borrow()` can panic, but non-panicing options like `try_borrow()` are also available. Cannot be shared between threads.
| <code>std::cell::[UnsafeCell](https://doc.rust-lang.org/std/cell/struct.UnsafeCell.html)&lt;T&gt;</code> | This exists, but you'll need to use unsafe code and raw pointers. Best of luck! This is probably the closest equivalent to C++'s `mutable` keyword.
| <code>std::sync::[Mutex](https://doc.rust-lang.org/std/sync/struct.Mutex.html)&lt;T&gt;</code> | Similar to `RefCell<T>`, but thread safe, and so you `lock()` it instead of `borrow()`ing it. Doing so multiple times from the same thread is less specified: It may deadlock, or it may panic. It may also panic if another thread paniced while holding onto the mutex.
| <code>std::sync::[RwLock](https://doc.rust-lang.org/std/sync/struct.RwLock.html)&lt;T&gt;</code> | Similar to `Mutex<T>`, only allowing multiple readers simultaniously.
| <code>std::sync::[atomic](https://doc.rust-lang.org/std/sync/atomic/index.html)::[AtomicUsize](https://doc.rust-lang.org/std/sync/atomic/struct.AtomicUsize.html)</code> | Atomic types also implement interior mutability so they can be shared between multiple threads.
