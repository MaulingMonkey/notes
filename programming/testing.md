## Many kinds of test
*   Unit Testing
*   Integration Testing - like Unit Testing, except you don't mock away all your dependencies via Dependency Injection hell
*   Fuzz Testing
*   Acceptance Testing
*   Usability Testing
*   Regression Testing
*   Internal QA
*   External QA
*   Manual Developer Testing
*   Performance Testing (Benchmarking, Profiling, ...)

## TDD pros/cons
*   Lots of overlap with integration testing, which I would generally prioritize.
*   Anyone who writes C++ containers without unit tests is a fucking idiot who hates their coworkers and punishes them with heisenbugs.
*   Anyone who writes gameplay/graphics code in safe/sound languages, where what constitutes "correct" behavior involves personal taste /
    judgement calls like "is it fun?" or "is it beautiful?", might consider skipping unit tests.  Said tests are often difficult to write,
    prone to false positives and false negatives, and waste more time than they save when what they *do* occasionally catch would be caught
    trivially and incidentally during normal manual testing.
    And I say this as the kind of crazy person who writes [unit tests of debugger integration](https://github.com/rust-lang/rust/pull/60970/files) and
    [regression tests cdb against their .natvis files](https://github.com/rust-lang/rust/pull/76390), among other shennanigans.  I will do the hard
    work *if it'll catch real problems before they end up in production*.

## Catch
*   Normal maps accidentally unpacking normals into the `[0..1]` range instead of `[-1..+1]` in a niche shader variant.
*   [O(scary) strlen behavior](https://nee.lv/2021/02/28/How-I-cut-GTA-Online-loading-times-by-70/)
*   Win8 apps "crashing" via `exit(3)` if you open the charm bar for &gt;10 seconds while spamming a specific internet connectivity check
*   OOMing after 2 hour soak tests / repeated level transitions in active gameplay sessions causing fatal memory fragmentation
