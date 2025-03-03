## Saturday February 1st, 2025

Testing crates [courtesy of @Ralith](https://discord.com/channels/676678179678715904/676685797524766720/1335372471548776500):
[`exhaust`](https://docs.rs/exhaust/),
[`arbitrary`](https://docs.rs/arbitrary/),
[`proptest`](https://docs.rs/proptest/)



## Sunday February 2nd, 2025

*   [Spaced repetition can allow for infinite recall](https://www.efavdb.com/memory%20recall)
    *   [HN Thread](https://news.ycombinator.com/item?id=42908041)
    *   [How much knowledge can human brain hold](https://supermemo.guru/wiki/How_much_knowledge_can_human_brain_hold) (supermemo.guru)
    *   [SuperMemo](https://en.wikipedia.org/wiki/SuperMemo) (Wikipedia)
    *   [Incremental reading](https://en.wikipedia.org/wiki/Incremental_reading) (Wikipedia)



## Monday February 3rd, 2025

I once again muse over the creation of a "layer 2"(?) WASI OS, and find myself [searching lib.rs for IPC crates](https://lib.rs/search?q=ipc).
The [OSI model](https://en.wikipedia.org/wiki/OSI_model) describes 7 networking layers &mdash;
Physical, Data link, Network, Transport, Session, Presentation, and Application &mdash;
which are then awkwardly used to describe e.g. Ethernet, IP, TCP, HTTP, SOAP, and whatever application specific JSON horrors might be embedded within.

I feel there might be room for a similar description of *Operating System* layers.
Describing things which are OS-*adjacent*, we have:
*   Motherboard firmware (modern UEFI-supporting BIOSes are bootloaders themselves, and run complicated GUI apps for hardware management, flashing firmware, etc.)
*   Bootloaders (micro OSes which exist only to allow the selection and running of "real" OSes?)
*   Hypervisors (micro OSes which exist mainly to manage the running of "real" OSes?)
*   "Operating Systems" (the meat and potatos that most people talk about such as Windows, Linux, etc.)
*   Device firmware (a modern hard drive might have an ARM core running Linux just to talk to the motherboard!)
*   Emacs! (while people joke, other user space software such as browsers *absolutely* roll their own hardware I/O for gamepads etc. which make the claim to "being an operating system" much less satirical - to say nothing of Chrome OS.)

In the world of lightweight VMs:
*   <https://opensource.microsoft.com/blog/2024/11/07/introducing-hyperlight-virtual-machine-based-security-for-functions-at-scale/>
*   <https://github.com/hyperlight-dev/hyperlight>

Alternatively, instead of numbering layers, other options include terms like:
*   "Platform" [as suggested by Discord](https://discord.com/channels/186813135263367169/1089980828848762921/1336157501120450580) to encompass Chrome, Emacs, etc. without the narrower impliciation of "Operating System".
*   Sub-OS
*   ~~Child Operating System~~
*   ~~Child System~~



## Tuesday February 4th, 2025

Dreamed (see [../../../private/journal/2025-02-04.md](../../../private/journal/2025-02-04.md).)

More possible ≈OS terms:
*   Layered OS
*   Layer OS
*   OS Layer
*   Shell System
*   System Shell
*   Desktop System
*   (IDE = Integrated Development Environment inspires:)
    *   Shell Environment
    *   Environment Layer
    *   Desktop Environment

Watched:
*   [Start a Timer, Make a Decision](https://www.youtube.com/watch?v=tnVQVyOUV1A) (Cortex Podcast)
    *   Time Tracking: Stats vs Intentionality



## Wednesday Feburary 5th, 2025

*   Rediscovered and shared [Juice it or lose it - a talk by Martin Jonasson & Petri Purho](https://www.youtube.com/watch?v=Fy0aCDmgnxg)
*   Stumbled across [Jeremiah Reid - Juice your Turns](https://www.youtube.com/watch?v=xSYVQc7cH-4) ([Roguelike Celebration](https://www.youtube.com/@roguelikecelebration)) finding the above.



## Thursday Feburary 6th, 2025

TODO:
*   Replace mouse
*   WASI Sub-OS Projects:
    *   Fancy console
    *   Terminal/shell that runs on fancy console
    *   Command line utils
    *   Script debugger
    *   Process manager
    *   Task scheduler (in the cron sense)
    *   Filesystem
    *   Settings
    *   ...
*   WASI Sub-OS CLIs:
    *   `show processes`
    *   `show process 12345`
    *   ...
*   WASI Sub-OS Apps:
    *   Editor
    *   Timer
    *   Calendar
    *   Clock
    *   ...



## Friday Feburary 14th, 2025

*   [Exposing concurrency bugs with a custom scheduler](https://lwn.net/Articles/1007689/) (lwn.net) ([HN Discussion](https://news.ycombinator.com/item?id=43045151))
*   [Application Verifier: Cuzz](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/application-verifier-tests-within-application-verifier#cuzz)
