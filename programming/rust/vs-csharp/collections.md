# Rust vs C#: Collections

### Sequences

| Rust                                                              | C#                                                                                                                                                |
| ------------------------------------------------------------------| --------------------------------------------------------------------------------------------------------------------------------------------------|
| [`(A, B)`](https://doc.rust-lang.org/std/primitive.tuple.html)    | <code>[(A, B)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-tuples)</code> (C#7)                        |
| [`(A, B)`](https://doc.rust-lang.org/std/primitive.tuple.html)    | <code>[KeyValuePair]\<A, B\></code>                                                                                                               |
| <code>[alloc]::[vec]::[Vec]\<T\></code>                           | <code>[System.Collections.Generic].[List]\<T\></code>                                                                                             |
| <code>[alloc]::[collections]::[VecDeque]\<T\></code>              | *N/A*
| <code>[alloc]::[collections]::[LinkedList]\<T\></code>            | <code>[System.Collections.Generic].[LinkedList](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlist-1)\<T\></code> |

[\[T\]]:                        https://doc.rust-lang.org/std/primitive.slice.html

[LinkedList]:                   https://doc.rust-lang.org/std/collections/struct.LinkedList.html
[Vec]:                          https://doc.rust-lang.org/alloc/vec/struct.Vec.html
[VecDeque]:                     https://doc.rust-lang.org/std/collections/struct.VecDeque.html

[collections]:                  https://doc.rust-lang.org/std/collections/index.html
[vec]:                          https://doc.rust-lang.org/alloc/vec/index.html

[System.Collections.Generic]:   https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic
[KeyValuePair]:                 https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.keyvaluepair
[List]:                         https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1
[LinkedList]:                   https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlist-1

### Associative

| Rust                                                                                                                      | C#                                                                                                                                                                |
| --------------------------------------------------------------------------------------------------------------------------| ------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <code>[alloc]::[collections]::[BTreeMap](https://doc.rust-lang.org/std/collections/struct.BTreeMap.html)\<K, V\></code>   | <code>[System.Collections.Generic].[SortedDictionary](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.sorteddictionary-2)\<K, V\></code>  |
| <code>[alloc]::[collections]::[BTreeSet](https://doc.rust-lang.org/std/collections/struct.BTreeSet.html)\<K\></code>      | <code>[System.Collections.Generic].[SortedSet](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.sortedset-1)\<K\></code>                   |
| <code>[std]::[collections]::[HashMap](https://doc.rust-lang.org/std/collections/struct.HashMap.html)\<K, V\></code>       | <code>[System.Collections.Generic].[Dictionary](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2)\<K, V\></code>              |
| <code>[std]::[collections]::[HashSet](https://doc.rust-lang.org/std/collections/struct.HashSet.html)\<K\></code>          | <code>[System.Collections.Generic].[HashSet](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.hashset-1)\<K\></code>                       |

### Miscellanious

| Rust                                                                                                                      | C#                                                                                                                                                        |
| --------------------------------------------------------------------------------------------------------------------------| ----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <code>[alloc]::[collections]::[BinaryHeap](https://doc.rust-lang.org/std/collections/struct.BinaryHeap.html)\<T\></code>  | <code>[System.Collections.Generic].[PriorityQueue](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.priorityqueue-2)\<T,P\></code>?|
| *Just use <code>[VecDeque]\<T\></code> and `pop_front`*                                                                   | <code>[System.Collections.Generic].[Queue](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.queue-1)\<T\></code>                   |
| *Just use <code>[Vec]\<T></code>, it's methods are already called `push`/`pop`*                                           | <code>[System.Collections.Generic].[Stack](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.stack-1)\<T\></code>                   |

[BinaryHeap]:   https://doc.rust-lang.org/std/collections/struct.BinaryHeap.html

### Slices and Views

| Rust                          | C#                                                                                            |
| ----------------------------- | ----------------------------------------------------------------------------------------------|
| <code>\&mut [\[T\]]</code>    | <code>System.[Span](https://learn.microsoft.com/en-us/dotnet/api/system.span-1)\<T\></code>   |




<!-- Rust -->
[alloc]:        https://doc.rust-lang.org/alloc/index.html
[core]:         https://doc.rust-lang.org/core/index.html
[std]:          https://doc.rust-lang.org/std/index.html
