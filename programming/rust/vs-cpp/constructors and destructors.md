# Rust vs C++: Constructors &amp; Destructors



## Constructors

Rust does not have constructors.
The equivalent in typical Rust code is a regular named `fn`.
This avoids various edge cases involving partially constructed objects in C++ (`Derived` classes temporarily have vtables that point to their base classes, what happens when an exception is thrown mid-constructor, etc.)
One moment you have a bunch of loose fields, the next you have a fully constructed instance.
Rust provides some standard traits that contain fns which are the common moral equivalent of some C++ constructors:

| Rust                                                          | C++ Equivalent                                    |
| --------------------------------------------------------------| --------------------------------------------------|
| <code>[Default]::[default]</code>                             | Default constructor                               |
| <code>[Clone]::[clone]</code>                                 | Copy constructor                                  |
| *Everything is implicitly `memcpy`-moveable in Rust.*         | Move constructor                                  |
| <code>[From]::[from]</code> or <code>[Into]::[into]</code>    | Non-explicit single-arg conversion constructors   |
| *Non-trait static function*                                   | Most other constructors                           |


```cpp
int main() {
    std::string default_;                   // invokes default constructor
    std::string copied = default_;          // invokes copy constructor
    std::string moved = std::move(copied);  // invokes move constructor
    std::string conversion = "A C string";  // invokes `std::string::string(const char*)`
}
```

```rust
fn main() {
    let default_ : String = Default::default();
    let copied = default_.clone();                  // invokes `<String as Clone>::clone()`
    let moved = copied;                             // will invalidate and prevent future access to `copied`
    let conversion : String = "A Rust &str".into(); // invokes `<&str as Into<String>>::into()`
}
```

The one thing that's a bit magical is <code>[Copy]</code>.
The compiler won't invalidate old copies of variables if the type is <code>[Copy]</code>, instead knowing it's okay to simply `memcpy` new instances into existance.
<code>[Copy]</code> requires <code>[Clone]</code> be implemented.

### Example Constructor Implementations: C++

```cpp
struct Color {
    uint8_t red     = 0;
    uint8_t green   = 0;
    uint8_t blue    = 0;
    uint8_t alpha   = 0;

    Color() = default;
    Color(const Color& other) = default;

    Color(uint8_t red, uint8_t green, uint8_t blue)
        : red(red)
        , green(green)
        , blue(blue)
        , alpha(0xFF)
    {}

    Color(uint8_t red, uint8_t green, uint8_t blue, uint8_t alpha)
        : red(red)
        , green(green)
        , blue(blue)
        , alpha(alpha)
    {}
};
```

### Example Constructor Implementations: Rust

```rust
#[derive(Default)]      // Default::default() ≈ moral equivalent to a default constructor
#[derive(Clone, Copy)]  // Clone::clone() ≈ moral equivalent to a copy constructor
struct Color {
    pub red:    u8,
    pub green:  u8,
    pub blue:   u8,
    pub alpha:  u8,
}

impl Color {
    pub fn from_rgb(red: u8, green: u8, blue: u8) -> Self {
        Self {
            red, green, blue,
            alpha: 0xFF,
        }
    }

    pub fn from_rgba(red: u8, green: u8, blue: u8, alpha) -> Self {
        Self { red, green, blue, alpha }
    }
}
```

Alternatively, if you wanted to manually implement <code>[Default]</code> and <code>[Clone]</code> instead of using <code>#\[[derive](https://doc.rust-lang.org/reference/attributes/derive.html)\(...\)\]</code>:

```rust
impl Default for Color {
    fn default() -> Self {
        Self { red: 0, green: 0, blue: 0, alpha: 0 }
    }
}

impl Clone for Color {
    fn clone(&self) -> Self {
        Self { red: self.red, green: self.green, blue: self.blue, alpha: self.alpha }
    }
}
```



## Destructors

Rust *does* have *destructors*.
<code>[core]::[ops]::[Drop]::[drop]</code> will be executed automatically when instances of a type implementing that fn go out of scope.
This does mean Rust code needs to worry about panicing while unwinding to handle a panic,
much like C++ code needs to worry about throwing exceptions while unwinding to handle an exception.

### Example Destructor: C++

```cpp
class FileWrapper {
    FILE* file = nullptr;
public:
    ~FileWrapper() {
        fclose(file);
    }
};

int main() {
    FileWrapper file = ...;
    // fclose(file.file); will execute at the end of main
}
```

### Example Destructor: Rust

```rust
struct FileWrapper {
    file:   *mut libc::FILE,
}

impl Drop for FileWrapper {
    fn drop(&mut self) {
        unsafe { libc::fclose(file) };
    }
}

fn main() {
    let file : FileWrapper = ...;
    // libc::fclose(file.file); will execute at the end of main
}
```



<!-- References -->

[core]:         https://doc.rust-lang.org/core/index.html
[ops]:          https://doc.rust-lang.org/std/ops/index.html

[Clone]:        https://doc.rust-lang.org/std/clone/trait.Clone.html
[Copy]:         https://doc.rust-lang.org/std/marker/trait.Copy.html
[Default]:      https://doc.rust-lang.org/std/default/trait.Default.html
[Drop]:         https://doc.rust-lang.org/std/ops/trait.Drop.html
[From]:         https://doc.rust-lang.org/std/convert/trait.From.html
[Into]:         https://doc.rust-lang.org/std/convert/trait.Into.html

[clone]:        https://doc.rust-lang.org/std/clone/trait.Clone.html#tymethod.default
[default]:      https://doc.rust-lang.org/std/default/trait.Default.html#tymethod.default
[drop]:         https://doc.rust-lang.org/std/ops/trait.Drop.html#tymethod.drop
[from]:         https://doc.rust-lang.org/std/convert/trait.From.html#tymethod.from
[into]:         https://doc.rust-lang.org/std/convert/trait.Into.html#tymethod.into
