# Rust vs C++: Iteration



## Iteration in C++

```cpp
int main() {
    for (auto element : iterable()) {
        // ...
    }
}
```

Is syntactic sugar for, approximately:

```cpp
int main() {
    auto && container = iterable();
    for (auto iterator = container.begin(), const end = container.end(); iterator != end; ++iterator) {
        auto && element = *iterator;
        // ...
    }
}
```

There are several problems with this:
-   What should `.end()` return for an infinitely iterable sequence?
-   Must a counter be implemented so `operator==` returns true for iterators that have advanced the same amount?
-   What happens when you compare iterators from different containers?  (Bugs.  Bugs happen.)
-   What happens when you `*iterator` an end iterator?  (Undefined behavior.)
-   What happens when you mutate `container`?  (The iterator is invalidated and undefined behavior occurs when you use it.)

The horrors of C++03 for-each macros in a pre-`auto` world are also worthy of special mention:
-   <https://www.artima.com/articles/conditional-love-foreach-redux>
-   <https://www.boost.org/doc/libs/1_86_0/doc/html/foreach.html>



## Iteration in Rust

```rust
fn main() {
    for element in iterable() {
        // ...
    }
}
```

Is syntactic sugar for, approximately:

```rust
fn main() {
    let mut iter = core::iter::IntoIterator::into_iter(iterable());
    while let Some(element) = core::iter::Iterator::next(&mut iter) {
        // ...
    }
}
```

I like this much better:
-   Iterators are self contained.  They need not implement comparison.
-   Pattern matching based `Option`s avoid runtime explosions on `*iterator` unless you explicitly `.unwrap()`.
-   Infinite sequences are trivial.
-   What happens when you mutate `container`?  Compile time error... unless you resorted to `RefCell`s.
