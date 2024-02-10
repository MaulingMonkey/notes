The introduction of types to code generally involves adding more code - at minimum to declare and define new types, and frequently much more to annotate the types in use.
[More code = more bugs](https://news.ycombinator.com/item?id=33566329), more bugs = bad, ergo type systems = bad?
Sometimes!  ...but they can pay off in other ways.

Type systems can be differentiated on a few different axises:
*   Static vs Dynamic vs Gradual (Typescript)
*   Weak vs Strong
*   Explicitly annotated vs inferred

In simple easily tested scripts, I tend to lean towards dynamic, weak, inferred types.
Types frequently just don't add terribly much value in this context.
The entire program and it's state are visible, and the bugs generally easily caught via testing, limiting the usefulness of types.

In MLOC+ native codebases, I lean towards static, strong, and explicitly annotated types.
Small memory corruption bugs can turn into multi-week heisenbug hunts, trawling through huge codebases for causes.
The program escapes by ability to read it all, and being able to enforce invariants through the type system allows me to simplify my reasoning about state -
both when debugging and when writing the code - allowing me to write simpler code and diagnose when it goes awry more easily.
The difficulty in *testing* code is also less painful when I can get feedback at compile time before testing.

### The program-global problems
Reasoning about program-global context, state, and invariants becomes more difficult as programs grow larger.
Global variables can cause nightmareish issues by being modifiable by an unreadably large body of code.
Static variables can be less painful, but can still cause similar issues when code acts differently depending on program-global state.
Memory corruption is less of a problem in smaller programs, because there's less code to investigate that could be to blame - smaller haystack to find the needle in.
Functional purists take things to the extreme: all relevant state is explicitly passed in and returned out,
meaning there is no undocumented implicit state that could modify the behavior of your code.

Globally callable functions can have similar issues to globally accessible variables.
Can the function be called from DllMain?  In what COM apartments?
Is it signal safe?  Recursion-safe?  Thread-safe?  With what shared data?  Perhaps only if certain locks are held?  In what contexts?
With what parameters?
POSIX defines a whole slew of globally callable functions, and goes into detail about which functions need to be safe in which contexts, and valid inputs/outputs.
Type systems can help somewhat here by enforcing valid inputs/outputs.

### People taking things too far
*   [`Month` is a bridge too far](https://news.ycombinator.com/item?id=33438348)<br>
    More potentially buggy code, doesn't catch any bugs I've ever written or seen except the most shallow and temporary bugs.
*   [Adding `red`, `green`, and `blue` types to 'catch' backwards entry when that wasn't the bug](https://news.ycombinator.com/item?id=27375861)<br>
    Also prevents other bug-avoiding patterns (such as looping instead of duplicating.)

### References
*   [Strong and weak typing](https://en.wikipedia.org/wiki/Strong_and_weak_typing) (en.wikipedia.org)
*   [Type system](https://en.wikipedia.org/wiki/Type_system) (en.wikipedia.org)
