For making custom extensions with commands:
*   https://marketplace.visualstudio.com/items?itemName=ryuta46.multi-command
*   https://github.com/ryuta46/vscode-multi-command
*   https://github.com/ryuta46/vscode-multi-command/blob/master/src/extension.ts

# Style

* [Adding italics support to your favourite VSCode theme](https://dev.to/salted-bytes/adding-italics-support-to-your-favourite-vscode-theme-2ec9) (dev.to)
* [Color Themes](https://code.visualstudio.com/docs/getstarted/themes) (code.visualstudio.com)
* [Semantic Highlight Guide](https://code.visualstudio.com/api/language-extensions/semantic-highlight-guide) (code.visualstudio.com)

```js
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
* `matklad.rust-analyzer`
* `ms-vscode.vscode-typescript-tslint-plugin`

# Settings

### `%USERPROFILE%\AppData\Roaming\Code\User\keybindings.json`

```js
// Place your key bindings in this file to override the defaultsauto[]
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

### `%USERPROFILE%\AppData\Roaming\Code\User\settings.json`

```js
{
    "editor.renderWhitespace": "boundary",
    "editor.autoClosingBrackets": "never",
    "editor.autoClosingQuotes": "never",
    "editor.cursorStyle": "line-thin",
    "workbench.tree.indent": 20,
    "workbench.editor.highlightModifiedTabs": true,
    "debug.allowBreakpointsEverywhere": true,
    "extensions.ignoreRecommendations": true,
    "references.preferredLocation": "view",
    "editor.lineHeight": 13,
    "editor.rulers": [80, 120, 160],
    "workbench.colorCustomizations": {
        "editorRuler.foreground": "#eee"
    },
    "[markdown]": {
        "editor.wordWrap": "off",
    },
    "[html]": {
        "editor.wordWrap": "off",
    },
    "[xml]": {
        "editor.wordWrap": "off",
    },
    "[javascript]": {
        "editor.wordWrap": "off",
    },
    "diffEditor.wordWrap": "off",
    "editor.wordWrap": "off",
    "editor.fontSize": 11,
    "editor.suggestSelection": "first",
    "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
    "workbench.colorTheme": "Default Light+",
    "files.associations": {
        "*.xml": "html",
        "*.json": "jsonc"
    },
    "editor.accessibilitySupport": "off",
    "java.home": "C:\\Program Files\\Android\\Android Studio\\jre",
    "java.configuration.checkProjectSettingsExclusions": false,
    "rust-analyzer.checkOnSave": false,  // bogus intellisense errors in problems pane
    "rust-analyzer.diagnostics.enable": false,
    "rust-analyzer.hover.actions.enable": false, // "goto XYZ" menu items when hovering over underlined identifiers
    "rust-analyzer.lens.enable": false,         // inline hints like "2 implementations"
    "rust-analyzer.rustfmt.overrideCommand": null,
    "editor.minimap.enabled": true,
    "workbench.editor.openSideBySideDirection": "down",
    "editor.semanticTokenColorCustomizations": {
        "rules": {
            "keyword.unsafe": {
                "foreground": "#ff0000",
                "fontStyle": "bold",
            },
        }
    },
    "[jsonc]": {
        "files.trimTrailingWhitespace": true,
    },
    "files.trimTrailingWhitespace": true,
    "docker.showStartPage": false,
    "terminal.integrated.cursorBlinking": true,
    "terminal.integrated.defaultProfile.windows": "Command Prompt",
    "diffEditor.ignoreTrimWhitespace": false,
    "rust-analyzer.inlayHints.renderColons": false,
    "rust-analyzer.imports.granularity.group": "module",
    "rust-analyzer.imports.prefix": "crate",
    "rust-analyzer.inlayHints.parameterHints.enable": false,
    "rust-analyzer.inlayHints.typeHints.enable": false,

    //"editor.parameterHints.enabled": false,
    //"rust-analyzer.callInfo.full": false,
    "html.autoCreateQuotes": false,
    "editor.minimap.showSlider": "always",
    "terminal.integrated.enablePersistentSessions": false,
    "editor.minimap.size": "fit",
    "editor.stickyScroll.enabled": true,
    "workbench.editor.enablePreview": false,
    "dotnet.codeLens.enableReferencesCodeLens": false,
    "terminal.integrated.hideOnStartup": "always",
    "chat.commandCenter.enabled": false,
    "files.readonlyInclude": {
        "C:/Program Files (x86)/**": true,
        "C:/Program Files/**": true,
        "C:/rustc/**": true,
        "C:/Users/*/.cargo/**": true,
        "C:/Users/*/.rustup/**": true,
    },
}
```
