# Rust vs C#: Delegates



| Rust                                                                              | C#                                                                                                                                |
| --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------|
| <code>[Arc]\<dyn [Fn]\([f32]\) -\> [i32]\></code>                                 | <code>System.[Func](https://learn.microsoft.com/en-us/dotnet/api/system.func-2?view=net-9.0)\<[float], [int]\></code>             |
| <code>[Arc]\<dyn [Fn]\([f32]\)\></code>                                           | <code>System.[Action](https://learn.microsoft.com/en-us/dotnet/api/system.action-1?view=net-9.0)\<[float]\></code>                |
| <code>type Foo = [Arc]\<dyn [Fn]\([f32]\) -\> [i32]\>;</code>                     | <code>[delegate](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/) [int] Foo([float] a);</code>       |
| <code>let foo : Foo = [Arc]::new(\|a\| { [println!]\("O hai"\); 42 });</code>     | <code>Foo foo = [delegate](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/how-to-declare-instantiate-and-use-a-delegate) ([float] a) { System.[Console].[WriteLine](https://learn.microsoft.com/en-us/dotnet/api/system.console.writeline?view=net-9.0#system-console-writeline(system-string))("O hai"); return 42; }</code>

My C# delegates tend to reference `this` in ways that will make equivalent code - if translated directly into Rust code referencing `self` - give the borrow checker absolute fits.
General solutions to this include:
-   Allowing the anonymous functions to capture deep copies instead of shallow references (for values which are immutable and unchanging.)
-   Passing whatever `this` was as an initial context parameter.
-   <code>[Arc]\<[RefCell]\<...\>\></code> spam crimes.



<!-- References -->

[Arc]:          https://doc.rust-lang.org/std/sync/struct.Arc.html
[Fn]:           https://doc.rust-lang.org/std/ops/trait.Fn.html
[RefCell]:      https://doc.rust-lang.org/std/cell/struct.RefCell.html

[f32]:          https://doc.rust-lang.org/std/primitive.f32.html
[i32]:          https://doc.rust-lang.org/std/primitive.i32.html
[println!]:     https://doc.rust-lang.org/std/macro.println.html

[float]:        https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/floating-point-numeric-types
[int]:          https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/integral-numeric-types
[Console]:      https://learn.microsoft.com/en-us/dotnet/api/system.console?view=net-9.0
