For making custom extensions with commands:
*   https://marketplace.visualstudio.com/items?itemName=ryuta46.multi-command
*   https://github.com/ryuta46/vscode-multi-command
*   https://github.com/ryuta46/vscode-multi-command/blob/master/src/extension.ts

# Style

* [Adding italics support to your favourite VSCode theme](https://dev.to/salted-bytes/adding-italics-support-to-your-favourite-vscode-theme-2ec9) (dev.to)
* [Color Themes](https://code.visualstudio.com/docs/getstarted/themes) (code.visualstudio.com)
* [Semantic Highlight Guide](https://code.visualstudio.com/api/language-extensions/semantic-highlight-guide) (code.visualstudio.com)

```json
// .vscode/settings.json
{
    "editor.semanticTokenColorCustomizations": {
        "rules": {
            "keyword.unsafe": {
                "foreground": "#ff0000",
                "fontStyle": "bold",
            },
        }
    }
}
```

* `Ctrl`+`Shift`+`P`: `Developer:  Inspect Editor Tokens and Scopes`
* `Ctrl`+`Shift`+`P`: `Developer:  Generate Color Scheme From Current Settings`

# Extensions

* `adelphes.android-dev-ext` - Android
* `ms-vscode.cpptools` - Native Debugger
* `msjsdiag.debugger-for-chrome`
* `firefox-devtools.vscode-firefox-debug`
* `vscjava.vscode-java-debug`
* `slevesque.vscode-hexdump`
* `redhat.java`
* `ms-vscode-remote.remote-wsl`
* `rust-lang.rust-analyzer`
* `ms-vscode.vscode-typescript-tslint-plugin`
* `ryanluker.vscode-coverage-gutters`

# Settings

### `%USERPROFILE%\AppData\Roaming\Code\User\keybindings.json` or `~/.config/Code - OSS/User/keybindings.json`

```json
[
    {
        "key": "ctrl+alt+p",
        "command": "workbench.action.tasks.runTask"
    },
    {
        "key": "ctrl+f1",
        "command": "workbench.action.tasks.runTask",
        "args": "help"
    },
    {
        "key": "ctrl+f7",
        "command": "workbench.action.tasks.runTask",
        "args": "check-file"
    },
]
```

### `%USERPROFILE%\AppData\Roaming\Code\User\settings.json` or `~/.config/Code - OSS/User/settings.json`

```json
{
    // I tend to toggle wordWrap on/off for some text files via Alt+V,W.
    // I don't want that setting applying to most code files, however, so I override those manually here.
    // I don't know if such nonsense still works.
    "[bat]":                                        { "editor.wordWrap": "off" },
    "[c]":                                          { "editor.wordWrap": "off" },
    "[cpp]":                                        { "editor.wordWrap": "off" },
    "[csharp]":                                     { "editor.wordWrap": "off" },
    "[css]":                                        { "editor.wordWrap": "off" },
    "[diff]":                                       { "editor.wordWrap": "off" },
    "[hlsl]":                                       { "editor.wordWrap": "off" },
    "[html]":                                       { "editor.wordWrap": "off" },
    "[ignore]":                                     { "editor.wordWrap": "off" },
    "[ini]":                                        { "editor.wordWrap": "off" },
    "[java]":                                       { "editor.wordWrap": "off" },
    "[javascript]":                                 { "editor.wordWrap": "off" },
    "[json]":                                       { "editor.wordWrap": "off" },
    "[jsonc]":                                      { "editor.wordWrap": "off" },
    "[jsonl]":                                      { "editor.wordWrap": "off" },
    //markdown]":                                   { "editor.wordWrap": "off" },
    "[makefile]":                                   { "editor.wordWrap": "off" },
    "[objective-c]":                                { "editor.wordWrap": "off" },
    "[objective-cpp]":                              { "editor.wordWrap": "off" },
    //plaintext]":                                  { "editor.wordWrap": "off" },
    "[powershell]":                                 { "editor.wordWrap": "off" },
    "[python]":                                     { "editor.wordWrap": "off" },
    "[ruby]":                                       { "editor.wordWrap": "off" },
    "[rust]":                                       { "editor.wordWrap": "off" },
    "[typescript]":                                 { "editor.wordWrap": "off" },
    "[yaml]":                                       { "editor.wordWrap": "off" },
    "[xml]":                                        { "editor.wordWrap": "off" },

    // I should try AI sometime.  However, I'd rather not be waterboarded with AI prompts.  Fuck off AI.  Fuck off!
    "C_Cpp.copilotHover":                           "disabled",                 // Fuck off AI
    "chat.agent.enabled":                           false,                      // Fuck off AI
    "chat.agent.maxRequests":                       0,                          // Fuck off AI
    "chat.commandCenter.enabled":                   false,                      // Fuck off AI
    "chat.detectParticipant.enabled":               false,                      // Fuck off AI
    "chat.disableAIFeatures":                       true,                       // Fuck off AI
    "chat.extensionTools.enabled":                  false,                      // Fuck off AI
    "chat.focusWindowOnConfirmation":               false,                      // Fuck off AI
    "chat.mcp.access":                              "none",                     // Fuck off AI
    "chat.promptFiles":                             false,                      // Fuck off AI
    "chat.setupFromDialog":                         false,                      // Fuck off AI
    "chat.titleBar.openInAgentsWindow.enabled":     false,                      // Fuck off AI
    "chat.titleBar.signIn.enabled":                 false,                      // Fuck off AI

    "debug.allowBreakpointsEverywhere":             true,                       // Was once necessary to set breakpoints in Rust when using the C/C++ debugger.
    "diffEditor.ignoreTrimWhitespace":              false,                      // Don't clutter diffs with indentation changes.
    "diffEditor.wordWrap":                          "off",                      // I find diffs unreadable with word wrap enabled.
    "docker.showStartPage":                         false,
    "dotnet.codeLens.enableReferencesCodeLens":     false,
    "editor.accessibilitySupport":                  "off",
    "editor.autoClosingBrackets":                   "never",                    // Auto-typing 1 character isn't worth ruining my existing muscle memory.
    "editor.autoClosingQuotes":                     "never",                    // Auto-typing 1 character isn't worth ruining my existing muscle memory.
    "editor.cursorStyle":                           "line-thin",                // Narrower than default "line"
    "editor.fontSize":                              11,                         // Smaller than default 14
    "editor.lineHeight":                            13,                         // More compact than default 15
    "editor.minimap.enabled":                       true,                       // Default?
    "editor.minimap.showSlider":                    "always",                   // no visibility of scroll area otherwise
    "editor.minimap.size":                          "fit",                      // vertical scaling
    //"editor.parameterHints.enabled":              false,                      // rust-analyzer `: ty` insets?
    "editor.renderWhitespace":                      "boundary",                 // Yes yes, I'm a weirdo.  Replaces spaces with (grey) dots.
    "editor.rulers":                                [80, 120, 160],             // I tend to wrap code around column 120, but the others help provide guidance/hints.
    "editor.semanticTokenColorCustomizations": { "rules": { "keyword.unsafe": { "foreground": "#ff0000", "fontStyle": "bold" } } }, // Make `unsafe { ... }` blocks more visible.
    "editor.stickyScroll.enabled":                  true,
    "editor.suggestSelection":                      "first",                    // I prefer consistent order over incredibly modal "recentlyUsed[ByPrefix]" which ruins muscle memory.
    "editor.wordWrap":                              "off",                      // ...is this what Alt+V,W toggles?
    "extensions.ignoreRecommendations":             true,                       // I find VS Code's auto-recommendations to be obnoxious noise (doesn't even use the project's `.vscode/extensions.json` recommendations?)
    "files.associations": { "*.xml": "html", "*.json": "jsonc" },               // XML override helps with xhtml.  JSON override stops whining about trailing commas ala vscode settings files.
    "files.readonlyInclude": { "C:/Program Files (x86)/**": true, "C:/Program Files/**": true, "C:/rustc/**": true, "C:/Users/*/.cargo/**": true, "C:/Users/*/.rustup/**": true }, // don't accidentally edit stdlib source when debugging
    "files.trimTrailingWhitespace":                 true,                       // One of the *few* bits of auto-formatting I allow.
    "git.blame.editorDecoration.enabled":           false,
    "git.blame.statusBarItem.enabled":              true,
    "html.autoCreateQuotes":                        false,                      // Auto-typing 1 character isn't worth ruining my existing muscle memory.
    "inlineChat.holdToSpeech":                      false,                      // Fuck off AI
    "inlineChat.lineNaturalLanguageHint":           false,                      // Fuck off AI
    "java.configuration.checkProjectSettingsExclusions": false,
    "java.home": "C:\\Program Files\\Android\\Android Studio\\jre",             // ...for help with the Java debugger extension?
    "references.preferredLocation":                 "view",                     // Peek mode just confuses me.
    //"rust-analyzer.callInfo.full":                false,
    "rust-analyzer.checkOnSave":                    false,                      // Avoids bogus intellisense errors in problems pane
    "rust-analyzer.diagnostics.enable":             false,                      // Avoids bogus intellisense errors in problems pane
    "rust-analyzer.hover.actions.enable":           false,                      // "goto XYZ" menu items when hovering over underlined identifiers
    "rust-analyzer.imports.granularity.group":      "module",                   // Personal style preference about `use x::y::z::{...}` auto-formatting.
    "rust-analyzer.imports.prefix":                 "crate",                    // "Internal" relative paths tend to be brittle during refactoring, so I prefer `crate::`.
    "rust-analyzer.inlayHints.parameterHints.enable":false,                     // Disables phantom `: type` inserts into source text.  If I want to see a type I'll hover it.
    "rust-analyzer.inlayHints.renderColons":        false,
    "rust-analyzer.inlayHints.typeHints.enable":    false,                      // Disables phantom `: type` inserts into source text.  If I want to see a type I'll hover it.
    "rust-analyzer.lens.enable":                    false,                      // Disable inline hints like "2 implementations"
    "rust-analyzer.rustfmt.overrideCommand":        null,                       // Fuck off rustfmt
    "scm.repositories.selectionMode":               "single",                   // How the middle section of the Source Control (Ctrl+Shift+G) panel works.
    "task.allowAutomaticTasks":                     "on",                       // Enable tasks marked `"runOptions": {"runOn": "folderOpen"}` or similar.  I mostly use them to auto-fetch on a per-repository basis.
    "terminal.integrated.cursorBlinking":           true,                       // What madman thought non-blinking cursors were appropriate?
    "terminal.integrated.defaultProfile.windows":   "Command Prompt",           // Fuck off powershell
    "terminal.integrated.enablePersistentSessions": false,                      // No, don't show me random arbitrary old terminal contexts.
    "terminal.integrated.hideOnStartup":            "always",                   // If I want the terminal eating screen estate I'll damn well open it myself thanks.
    "terminal.integrated.initialHint":              false,                      // Fuck off AI
    "update.showReleaseNotes":                      false,
    "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
    "workbench.colorCustomizations": { "editorRuler.foreground": "#eee" },    // I like my column guides to be nearly invisible (#EEE on #FFF)
    "workbench.colorTheme":                         "Light+",                   // Of course, this is assuming a certain style.
    "workbench.editor.empty.hint":                  "hidden",                   // Fuck off AI
    "workbench.editor.enablePreview":               false,                      // Preview mode is just weird modal nonsense IME.  If I open a file, I wanted it open, not closing automagically behind me!
    "workbench.editor.highlightModifiedTabs":       true,
    "workbench.editor.openSideBySideDirection":     "down",                     // How the editor auto-splits when tapping Ctrl+2 etc.
    "workbench.secondarySideBar.defaultVisibility": "hidden",                   // Fuck off AI?
    "workbench.tree.indent":                        20,                         // Explorer View (Ctrl+Shift+E) child item indentation.  Up from default of 8, which is a bit hard to read IME.
}
```
