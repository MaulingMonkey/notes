# Tuesday December 24th, 2024

*Chrome* recently yeeted support for *uBlock Origin*.
This gives me the choice of *uBlock Origin Lite* or *Firefox*.
My bookmarks tie me to a single browser at a time.

Tempting to author `bookmarks2md`.
Browser bookmarks tend to be stored in `.json`, with `sqlite3` favicon caches, possibly with compression such as lz4 on top.
v1 can skil favicons - just having a list would already be a boon.
Searching lib.rs, most "bookmark" crates deal with folders/paths, not urls.
<https://lib.rs/crates/bogrep> is an exception.



# Thursday December 26th, 2024
Did end up authoring [`bookmarks2md`](https://github.com/MaulingMonkey/bookmarks2md).
Exported all bookmarks to markdown and saved to private git notes.
Did some preliminary exports to public git notes.
Ended up not bothering with favicons thus far.

Firefox has not been handling my youtube needs well.
