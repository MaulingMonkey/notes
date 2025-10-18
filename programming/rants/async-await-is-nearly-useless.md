# `async`/`await` is nearly useless

When C# first introduced `async`/`await`, I thought it was really neat and looked forward to using it everywhere.
I then discovered ≈none of my use cases fit well with the faux-linear/procedural model and went on to use it approximately never.



## Use Case: Asset Loading

Loading assets such as textures from disk is an example of something to handle asyncronously.

Performing I/O in your rendering loop *will* cause horrific framerate stutters and other such problems.

However, `async`/`await` don't really help much with writing all the boilerplate:
-   Fading in minimap icons etc. when they're loaded instead of blocking the game loop
-   Rendering using lower resolution mipmaps when possible
-   Throttling the amount of actual file requests to avoid e.g. `EMFILE` / `ENFILE` errors due to running out of file descriptors
-   Using packfiles and shared file handles to reduce OS overhead vs file-per-asset models
-   Using high-throughput OS-native asyncronous I/O systems
-   Offloading to DMA hardware
-   Prioritizing blocking / gameplay-critical assets over delayable UI garnish
-   Preloading based on game state
-   Unloading unneeded assets to fit within memory budgets



## Use Case: GPU Queries

Buses and queues introduce latency - you don't want to stall your game waiting for the result of an occlusion query, or reading a screenshot back from the GPU.
There's a good amount of graphics API specific nonsense to go around, but very little benefit from `async`/`await` IME.



## Use Case: Actors / AI / Game Entities / UI

Trying to contort these to the linear / stack-based `async` programming model is an exercise in pain and fustration IME.
State machines and other "traditional" solutions to asyncronous programming seem to work much better?



## Use Case: JavaScript

JS is full of async-only APIs that cannot be converted into blocking requests due to browser limitations.
In this context, `async`/`await` are *slightly* useful.



## References

-   <https://learn.microsoft.com/en-us/dotnet/csharp/asynchronous-programming/>
-   <https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Async_JS>
