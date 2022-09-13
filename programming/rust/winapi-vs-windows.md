| feature       | winapi                                                             | windows |
| ------------- | ------------------------------------------------------------------ | ------- |
| safety        | ⚠️ everything is `unsafe`, which is at least sound                | ❌ soundness issues like [windows-rs#1409](https://github.com/microsoft/windows-rs/issues/1409) |
| api stability | ✔️ fairly stable 0.3.x                                            | ❌ 0.28.x and continuing to skyrocket
| codegen       | ⚠️ manual                                                         | ⚠️ from `*.winmd` metadata |
| coverage      | ⚠️ decent coverage of Win32, but missing some stuff               | ✔️ everything under the sun, including new UWP APIs
| docs          | ⚠️ [docs.rs](https://docs.rs/winapi/) at least has all symbols    | ⚠️ [docs.rs](https://docs.rs/windows-sys/) works for windows<strong>-sys</strong><br>⚠️ [github pages](https://microsoft.github.io/windows-docs-rs/doc/windows/) I can't remember the URL of, for windows
| maintainence  | ⚠️ pull requests backlogged                                       | ✔️ responsive last I checked |
| api design    | ⚠️ 1:1 match of headers and their mishandled constness            | ⚠️ drowning in traits and newtypes that add little value
