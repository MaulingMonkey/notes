# Rust vs C++: Command Line Arguments

We'll be making a little `cat` or `type` style program as an example.
This will [Mojibake](https://en.wikipedia.org/wiki/Mojibake) if e.g. the input file is UTF-8 and the output console isn't.
Consider showing an alternatve that doesn't?



## Rust

TODO: compile/test

```rust
use std::ffi::OsString;
use std::fs::File;
use std::path::PathBuf;
use std::process::exit;

fn main() {
    let mut args = std::env::args_os(); // Available from anywhere
    let exe = PathBuf::from(args.next().unwrap_or_else(|| OsString::from("cat")));

    // Handles paths on Windows containing UTF-16 - even malformed UTF-16 - flawlessly.
    let path = PathBuf::from(args.next().unwrap_or_else(||{
        eprintln!("Usage: {exe} [filename]");
        exit(1)
    }));

    let mut file = File::open(&path).unwrap_or_else(|err|{
        eprintln!("Error: Unable to open {path:?}: {err}");
        exit(1)
    });

    // N.B. this will Mojibake if the file is UTF-8 but the Console isn't (and it usually isn't.)
    std::io::copy(&mut file, &mut std::io::stdout()).unwrap_or_else(|err|{
        eprintln!("Error: Unable to finish reading {path:?}: {err}");
        exit(1)
    });

    exit(0);
}
```



## C++

TODO: compile/test

```cpp
#include <iostream> // std::{cout, cerr}
#include <cerrno>   // errno
#include <cstdio>   // std::{fopen, ...}
#include <cstring>  // std::strerror

int main(int argc, char** argv) { // Magic arguments to main - globals such as __argv may exist but are nonstandard
    if (argc < 2) {
        const auto exe = (argc < 1) ? "cat" : argv[0];
        std::cerr << "Usage: " << exe << " [filename]\n";
        return 1;
    }

    // Wildly mishandles *well formed* UTF-16 paths on Windows!
    const auto path = argv[1];
    const auto file = std::fopen(path, "r");
    if (!file) {
        const int err = errno;
        std::cerr << "Error: Unable to open " << path << ": " << err << " " << std::strerror(err) << "\n";
        return 1;
    }

    // N.B. this will Mojibake if the file is UTF-8 but the Console isn't (and it usually isn't.)
    char buffer[1024];
    for (int read; read=fread(file, 1, buffer, sizeof(buffer));) {
        std::fwrite(buffer, 1, read, stdout); // XXX: Don't treat writing to stdout as an error
    }
    if (const int err = std::ferror(file)) {
        std::cerr << "Error: Unable to finish reading " << path << ": " << err << " " << std::strerror(err) << "\n";
        return 1;
    }

    return 0;
}
```



## C++23?

TODO:
-   std::filesystem::path nonsense?
