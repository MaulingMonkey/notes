# Fighting The Borrow Checker

I spent *most* (95%+?) of my "fighting the borrow checker" time writing code I would never try to write in C++. A simple example is strings: I'd spend a lot of time trying to get a &str to work instead of a String::clone, where in equivalent C++ code I'd never use std::string_view over std::string - not because it would be a memory error to do so in my code as it stood, but because it'd be nearly impossible to keep it memory safe with code reviews and C++'s limited static analysis tooling.

This was made all the worse by the fact that I frequently, eventually, *succeeded* in "winning". I would write unnecessary and unprofiled "micro-optimizations" that I was confident were safe *and would remain safe* in Rust, that I'd never dare try to maintain in C++.

Eventually I mellowed out and started .clone()ing when I would deep copy in C++. Thus ended my fight with the borrow checker.

# Avoidance Techniques

-   <code>[Clone]::[clone]</code>
-   Use indicies, handles, or identifiers instead of pointers
-   <code>[Box]::[leak]</code> to `'static` lifetime
    -   E.g. it's sane for a compiler to have access to the source code for the duration of the program
    -   Such explicit leaks are easily tracked

<!-- References -->
[Box]:                  https://doc.rust-lang.org/std/boxed/struct.Box.html
[Clone]:                https://doc.rust-lang.org/std/clone/trait.Clone.html
[clone]:                https://doc.rust-lang.org/std/clone/trait.Clone.html#tymethod.clone
[leak]:                 https://doc.rust-lang.org/std/boxed/struct.Box.html#method.leak
