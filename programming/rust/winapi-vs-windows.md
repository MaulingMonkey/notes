| feature       | winapi                                                             | windows |
| ------------- | ------------------------------------------------------------------ | ------- |
| safety        | ⚠️ everything is `unsafe`, which is at least sound                | ❌ soundness issues like [windows-rs#1409](https://github.com/microsoft/windows-rs/issues/1409) |
| api stability | ✔️ fairly stable 0.3.x                                            | ❌ 0.28.x and continuing to skyrocket
| codegen       | ⚠️ manual                                                         | ⚠️ from `*.winmd` metadata |
| coverage      | ⚠️ decent coverage of Win32, but missing some stuff               | ✔️ everything under the sun, including new UWP APIs
| docs          | ⚠️ [docs.rs](https://docs.rs/winapi/) at least has all symbols    | ❌ [github pages](https://microsoft.github.io/windows-docs-rs/doc/windows/) I can never remember the URL for |
| maintainence  | ⚠️ pull requests backlogged                                       | ✔️ probably responsive? |