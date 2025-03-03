## Thursday Februrary 24th, 2022
*   Experimented with lib[clang](https://docs.rs/clang/), [syn](https://docs.rs/syn/)
*   Deployed [Syncthing](https://syncthing.net/).  It seems to work okay for sync.  Less convinced about backup.
*   Released [winresult](https://docs.rs/winresult/)
*   Worked a ton on [thindx](https://docs.rs/thindx/)
*   Worked on a hwnd-focused competitor to [winsafe](https://docs.rs/winsafe/)
*   Filed soundness issues against winsafe
*   [🦀 raw-git-experiments 🦀](https://github.com/MaulingMonkey/raw-git-experiments)

### Looked into compiling rustc for wasm
*   <https://github.com/rust-lang/miri/issues/722>
*   <https://github.com/RReverser/rust/tree/compile_rustc_for_wasm>

### TODO projects not of note
*   Asset Pack Theorycrafting
    *   Example Use Case: <https://thoseawesomeguys.com/prompts/>
    *   Example Use Case: Bitmap fonts
    *   Crate that embeds .png data?
    *   Subcommand that auto-downloads/caches?  From <https://opengameart.org/>?
*   Spritesheet Slicing
*   Scrape HTML to appropriate .zip / .cbr format
*   Create no-gc, custom ignore, git tree without git/libgit for backup?
    *   1 branch = 1 machine
    *   1 commit = 1 snapshot
    *   Look into commit signing for p2p distribution validation?
*   Tray icon
    *   Monthly backups
    *   Sync
*   Experiment with LAN broadcasting p2p chat
*   Experiment with rusttls for encryption
