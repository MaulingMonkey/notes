# Rust vs C++: Containers



## Types

These aren't ABI compatible, but are roughly equivalent semantically.

### Results/Options

| Rust                                                                      | C++                                               |
| --------------------------------------------------------------------------| --------------------------------------------------|
| <code>[core]::[option]::[Option]</code>                                   | <code>[std::optional]</code> (C++17)              |
| <code>[core]::[result]::[Result]</code>                                   | <code>[std::expected]</code> (C++23)              |
| <code>[core]::[result]::[Result]::[Err]</code>                            | <code>[std::unexpected]</code> (C++23)            |
| <code>[core]::[result]::[Result]::[Ok]</code>                             | No extra type/wrapper                             |

[option]:               https://doc.rust-lang.org/core/option/index.html
[result]:       https://doc.rust-lang.org/core/result/index.html

[Option]:       https://doc.rust-lang.org/std/option/enum.Option.html
[Result]:       https://doc.rust-lang.org/std/result/enum.Result.html

[Ok]:                   https://doc.rust-lang.org/std/result/enum.Result.html#variant.Ok
[Err]:                  https://doc.rust-lang.org/std/result/enum.Result.html#variant.Err

[std::optional]:        https://en.cppreference.com/w/cpp/utility/optional
[std::expected]:        https://en.cppreference.com/w/cpp/utility/expected
[std::unexpected]:      https://en.cppreference.com/w/cpp/utility/expected/unexpected

### Sequences

| Rust                                                                                                                      | C++                                               |
| --------------------------------------------------------------------------------------------------------------------------| --------------------------------------------------|
| <code>let variable : [\[T; 3\]](https://doc.rust-lang.org/std/primitive.array.html);</code>                               | <code>T variable[3];</code>                       |
| [`[T; 3]`](https://doc.rust-lang.org/std/primitive.array.html)                                                            | <code>[std::array]\<T, 3\></code> (C++11)         |
| [`(A, B)`](https://doc.rust-lang.org/std/primitive.tuple.html) <sup>(see also [rust/vs-cpp/tuples.md](tuples.md))</sup>   | <code>[std::pair]\<A, B\></code>                  |
| [`(A, B)`](https://doc.rust-lang.org/std/primitive.tuple.html) <sup>(see also [rust/vs-cpp/tuples.md](tuples.md))</sup>   | <code>[std::tuple]\<A, B\></code> (C++11)         |
| <code>[alloc]::[vec]::[Vec]\<T\></code>                                                                                   | <code>[std::vector]\<T\></code>                   |
| <code>[alloc]::[vec]::[Vec]\<T, Allocator\></code>    <sup>\[1\]</sup>                                                    | <code>[std::vector]\<T, Allocator\></code>        |
| <code>[alloc]::[collections]::[VecDeque]\<T\></code>                                                                      | <code>[std::deque]\<T\></code>                    |
| <code>[alloc]::[collections]::[LinkedList]\<T\></code>                                                                    | <code>[std::list]\<T\></code>                     |
| *N/A in the standard library*                                                                                             | <code>[std::forward_list]\<T\></code> (C++11)     |
| *N/A in the standard library*                                                                                             | <code>[std::inplace_vector]\<T, 3\></code> (C++26)|

1.  Allocator APIs require nightly Rust

[LinkedList]:   https://doc.rust-lang.org/std/collections/struct.LinkedList.html
[Vec]:          https://doc.rust-lang.org/alloc/vec/struct.Vec.html
[VecDeque]:     https://doc.rust-lang.org/std/collections/struct.VecDeque.html

[collections]:          https://doc.rust-lang.org/std/collections/index.html
[vec]:                  https://doc.rust-lang.org/alloc/vec/index.html

[std::array]:           https://en.cppreference.com/w/cpp/container/array
[std::deque]:           https://en.cppreference.com/w/cpp/container/deque
[std::forward_list]:    https://en.cppreference.com/w/cpp/container/forward_list
[std::inplace_vector]:  https://en.cppreference.com/w/cpp/container/inplace_vector
[std::list]:            https://en.cppreference.com/w/cpp/container/list
[std::pair]:            https://en.cppreference.com/w/cpp/utility/pair
[std::tuple]:           https://en.cppreference.com/w/cpp/utility/tuple
[std::vector]:          https://en.cppreference.com/w/cpp/container/vector

### Associative

| Rust                                                                      | C++                                               |
| --------------------------------------------------------------------------| --------------------------------------------------|
| <code>[alloc]::[collections]::[BTreeMap]\<K, V\></code>                   | <code>[std::map]\<K, V\></code>                   |
| <code>[alloc]::[collections]::[BTreeSet]\<K\></code>                      | <code>[std::set]\<K\></code>                      |
| <code>[std]::[collections]::[HashMap]\<K, V\></code>                      | <code>[std::unordered_map]\<K, V\></code> (C++11) |
| <code>[std]::[collections]::[HashSet]\<K\></code>                         | <code>[std::unordered_set]\<K\></code> (C++11)    |
| *N/A in the standard library*                                             | <code>[std::multiset]</code>                      |
| *N/A in the standard library*                                             | <code>[std::multimap]</code>                      |
| *N/A in the standard library*                                             | <code>[std::unordered_multiset]</code>            |
| *N/A in the standard library*                                             | <code>[std::unordered_multimap]</code>            |
| *N/A in the standard library*                                             | <code>[std::flat_set]\<K\></code>                 |
| *N/A in the standard library*                                             | <code>[std::flat_map]\<K, V\></code>              |
| *N/A in the standard library*                                             | <code>[std::flat_multiset]\<K\></code>            |
| *N/A in the standard library*                                             | <code>[std::flat_multimap]\<K, V\></code>         |

[HashMap]:      https://doc.rust-lang.org/std/collections/struct.HashMap.html
[HashSet]:      https://doc.rust-lang.org/std/collections/struct.HashSet.html
[BTreeMap]:     https://doc.rust-lang.org/std/collections/struct.BTreeMap.html
[BTreeSet]:     https://doc.rust-lang.org/std/collections/struct.BTreeSet.html

[std::map]:                 https://en.cppreference.com/w/cpp/container/map
[std::set]:                 https://en.cppreference.com/w/cpp/container/set
[std::multiset]:            https://en.cppreference.com/w/cpp/container/multiset
[std::multimap]:            https://en.cppreference.com/w/cpp/container/multimap
[std::flat_set]:            https://en.cppreference.com/w/cpp/container/flat_set
[std::flat_map]:            https://en.cppreference.com/w/cpp/container/flat_map
[std::flat_multiset]:       https://en.cppreference.com/w/cpp/container/flat_multiset
[std::flat_multimap]:       https://en.cppreference.com/w/cpp/container/flat_multimap
[std::unordered_map]:       https://en.cppreference.com/w/cpp/container/unordered_map
[std::unordered_set]:       https://en.cppreference.com/w/cpp/container/unordered_set
[std::unordered_multiset]:  https://en.cppreference.com/w/cpp/container/unordered_multiset
[std::unordered_multimap]:  https://en.cppreference.com/w/cpp/container/unordered_multimap

### Miscellanious

| Rust                                                                              | C++                                               |
| ----------------------------------------------------------------------------------| --------------------------------------------------|
| <code>[alloc]::[collections]::[BinaryHeap]\<T\></code>                            | <code>[std::priority_queue]\<T\></code>           |
| *Just use <code>[VecDeque]\<T\></code> and `pop_front`*                           | <code>[std::queue]\<T\></code>                    |
| *Just use <code>[Vec]\<T></code>, it's methods are already called `push`/`pop`*   | <code>[std::stack]\<T\></code>                    |

[BinaryHeap]:   https://doc.rust-lang.org/std/collections/struct.BinaryHeap.html

[std::priority_queue]:  https://en.cppreference.com/w/cpp/container/priority_queue
[std::queue]:           https://en.cppreference.com/w/cpp/container/queue
[std::stack]:           https://en.cppreference.com/w/cpp/container/stack

### Slices and Views

| Rust                                                                      | C++                                               |
| --------------------------------------------------------------------------| --------------------------------------------------|
| <code>&\[T\]</code>                                                       | <code>[std::span]\<T\></code> (C++20)         |
| <code>&\[T; 3\]</code>                                                    | <code>[std::span]\<T, 3\></code> (C++20) <br> <code>T (&)[3]</code>   |
| *N/A in the standard library*                                             | <code>[std::mdspan]\<T, ...\></code> (C++23)                  |

[std::mdspan]:          https://en.cppreference.com/w/cpp/container/mdspan
[std::span]:            https://en.cppreference.com/w/cpp/container/span



<!-- Rust -->
[alloc]:        https://doc.rust-lang.org/alloc/index.html
[core]:         https://doc.rust-lang.org/core/index.html
[std]:          https://doc.rust-lang.org/std/index.html
