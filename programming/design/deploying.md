A [discord discussion](https://discord.com/channels/273534239310479360/335502067432947748/1336579142669566045) in the "Rust Programming Language Community" discord server.

#### imbris 🌧 — 2/4/25, 10:08 PM

Are there any good tools for syncing build results to run on a device where compilation would be much slower? This would be in the context of doing iterative development with change-compile-run cycles to e.g. fix device specific bugs.

The destination is running Windows in most cases where I want this.



#### MaulingMonkey — 2/4/25, 11:32 PM

IME deploys tend to be incredibly platform and codebase specific... but also pretty simple command line nonsense, to the point where I probably wouldn't try to write a generic crate, because it's too easy to write what I need, and too hard to write what everyone else would need in the generic sense.

> Deploying to and debugging on a Windows target instead of compiling directly on said target

My usual approach here would be:

1.  Deploy core binaries or packages and symbols using network shares.
    -   Option A: Push model:  Create a network share directly on the target device, create a `deploy.bat` on the development machine that mostly just calls `robocopy` with the appropriate paths and flags (possibly sprinkling in `mkdir`s, `copy`s, `xcopy`s etc.)
        -   Linux edition: Create a `deploy.sh`, use `scp` to copy over ssh instead (example: <https://github.com/MaulingMonkey/rust-opendingux-test/blob/master/scripts/deploy.sh>)
    -   Option B: Pull model: Create a network share on the development machine, create an `update.bat` or similar on the target machine that mostly just calls `robocopy` with the appropriate paths and flags (possibly sprinkling in `mkdir`s, `copy`s, `xcopy`s etc.)
        -   Can be nicer if, instead of a development machine, you have build servers you want to pull from.

2.  \[Optional\] For development builds, the executable might pull assets or other heavyweight data lazily on-demand from the development machine so I don't have to sync all 200GB+ of the yet-to-be-optimized assets on large gamedev projects as part of step 1.
    -   Simplest version of this is, again, to abuse network shares.
    -   More complicated versions might be HTTP, HTTPS, custom protocol, etc. and might communicate with whatever custom dev tools you have on your dev machine, possibly also sending back logs, recieving debug commands, etc.

3.  Also consider setting up [Remote Debugging](https://learn.microsoft.com/en-us/visualstudio/debugger/remote-debugging?view=vs-2022) if you've got some terrible windows laptop/tablet target, and would rather debug on your quad monitor dev machine than install VS on the laptop/tablet.



#### MaulingMonkey — 2/4/25, 11:41 PM

My most complicated *publicly available* example of 1A for Windows is probably here: <https://github.com/MaulingMonkey/thindx/blob/725fcca59c28a15dbebd8e1c3a95a68b554022a8/xtask/pages.rs#L46>
-   Written in Rust, not batchfile, because it got complicated enough to warrant that I guess.
-   Deploys docs, not binaries, but that doesn't matter much.
-   I also have my "deploy" optionally build stale stuff.
-   Creates a git commit in a worktree for `gh-pages` and pushes it to a remote, instead of just pushing to a network share.
-   *Does* use robocopy (I even documented my favorite flags! see <https://ss64.com/nt/robocopy.html> too tho.)
-   Opens deployed docs (roughly equivalent to a deploy+run workflow)

Other customizations you might consider:
-   Kill the running process, if any, as part of your deploy script, so your deploy won't fail due to locked files.
-   Explicit commands to wait for a deployment to succeed, or to validate the deployment went OK.
    -   Probably not a huge issue with windows, but for e.g. Android, tooling would claim to install an apk but fail to actually update it.  So I had to write my own that validated a deploy actually deployed.  [../platforms/android.md#workarounds-and-androidsamsung-bugs](../platforms/android.md#workarounds-and-androidsamsung-bugs)

> `robocopy`

N.B. robocopy has the obnoxious behavior of using non-zero exit codes for many types of success.



#### MaulingMonkey — 2/4/25, 11:59 PM
> More complicated versions might be HTTP, HTTPS, custom protocol, etc. and might communicate with whatever custom dev tools you have on your dev machine

One of the reasons you might do this instead of a simple network share, is to allow e.g. running a game before you've fully built all assets and shaders.

If your game then requests e.g. a shader your development machine hasn't compiled yet, that request can cause your dev tools that are building assets to bump compiling that shader up in the priority queue / shove it into the front of said queue.
