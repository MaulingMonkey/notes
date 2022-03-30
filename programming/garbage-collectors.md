## Context

I'm a game developer.  I work on games, engines, editors, tools, plugins for third party tools, PC, consoles, handhelds, etc etc etc...

## The Good

Garbage collection is fine - great even - for:
* Scripts or batch processes where the occasional GC spike isn't an issue.<br>Compiling shaders might take several seconds, minutes - what's a few GC pauses between friends while doing so?
* Editor logic that's not part of the render loop
* Dev machines and servers where you can just throw more RAM at the problem
* Some lock free algorithms and containers
* In small constrained doses where the live objects set doesn't balloon into huge spikes running GC finalizers/compaction/sweeps.

## The Bad

Broad, language-level garbage colleciton is a nightmare for game logic and rendering.  I have hard constraints:

* Framerate
    * To achieve 30fps, I must complete a frame within 33ms.
    * To achieve 60fps, I must complete a frame within 16ms.
    * To avoid nausea in VR, I've read 90fps is a must.
    * I've seen 1ms in the wrong place at the wrong time be the difference between smooth rendering and unacceptable stuttering when I miss vsync.
    * I've seen 100ms+ pauses from garbage collectors on modern hardware despite decades of claims that "GCs are much better these days".
* Memory
    * Xbox 360 had 512MB
    * Xbox One has 8GB
    * Build servers had 12GB
    * Dev machines had 32GB
    * I have seen projects run out of memory on *all of these systems*, and GCs have contributed to many of these OOMs.
    * Hard limits on total texture memory etc.
    * *Sometimes* you have pagefiles, but typically you don't.
* Diagnostics
    * It's trivial to figure out what's churning through `free()`s in a traditional profiler, tracers, or even printf style timestamp "profiling".
    * It's near impossible to figure out what's causing GC churn/spikes on a large project without language-specific profilers, statistics, or callbacks.
* Mitigation
    * Object pooling can reduce GC frequency, but not GC duration (set of live objects remains large), which means you must pool and preallocate *everything* to completely avoid allocation to avoid potentially triggering GC.
    * Languages with GCs typically have poor tools for manual memory management.


## Horror Story: Squirrel

I was working on a game that embedded [squirrel](http://squirrel-lang.org/).
We occasionally had complicated ownership chains:
C++ objects referenced squirrel objects referenced C++ objects referenced squirrel objects referenced C++ objects.
A "full" garbage collection pass would end up releasing some C++ objects, which would unroot some Squirrel objects.
This occasionally meant a full garbage collection when switching levels wouldn't unreference the root level object containing the entire world.
This would leak several hundred megabytes on a console with only 512MB total.  Sometimes.  Depending on how recently a previous GC was run.
This would crash the program OOM.
The horrible bodge to fix it?  Explicitly garbage collect multiple times.
The alternative?  Fail certification, retest, release late and ruin who knows how many marketing plans.

## Horror Story: duktape

I was working on a game that embedded [duktape](https://duktape.org/), a JavaScript engine.
Every 30 seconds, the game would pause for 100ms.  Unacceptable.
Profiling revealed it was garbage collection... and not much else, since all objects go through the exact same codepath.
JavaScript's lack of explicit static types means even writing our own tooling to enumerate duk_heap before-and-after GC wouldn't help much.
What we were able to figure out from what we could investigate was that it wasn't the fault of any one single thing, but a death by a thousand cuts.
We were second-guessing our use of duktape anyways, so we delayed working on the problem until the entire project was canceled.

## Horror Stories: Reference Cycles

Explicit allocation isn't a panacea.
I've implemented my share of memory debugging tools to help track down e.g. cycles of shared_ptr s or codebase specific equivalents.
However, they tend to be more predictable and incrementally improvable.

## More Funky GC stuff

* [Go memory ballast: How I learnt to stop worrying and love the heap](https://blog.twitch.tv/en/2019/04/10/go-memory-ballast-how-i-learnt-to-stop-worrying-and-love-the-heap/)
    * Allocating 10GB to stop GCing 10 times per second in an effort to reduce the 30% overhead of GC is the kind of cargo culting bullshit that arises from not having real control over your allocator.
* [The Broken Promises of MRI/REE/YARV](http://timetobleed.com/the-broken-promises-of-mrireeyarv/)
