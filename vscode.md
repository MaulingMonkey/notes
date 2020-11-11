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

# Settings.json

* `%USERPROFILE%\AppData\Roaming\Code\User\settings.json`

```js
{
    "editor.renderWhitespace": "boundary",
    "editor.autoClosingBrackets": "never",
    "editor.autoClosingQuotes": "never",
    "editor.cursorStyle": "line-thin",
    "workbench.tree.indent": 20,
    "workbench.editor.highlightModifiedTabs": true,
    "workbench.editor.enablePreview": false,
    "debug.allowBreakpointsEverywhere": true,
    "search.usePCRE2": true,
    "terminal.integrated.fontSize": 12,
    "extensions.ignoreRecommendations": false,
    "references.preferredLocation": "view",
    "window.zoomLevel": 2,
    "terminal.integrated.shell.windows": "C:\\Windows\\System32\\cmd.exe",
    //"rust.rls": {
    //    "revealOutputChannelOn": "info",
    //},
    //"rust.mode": "rls",
    //"rust-client.channel": "beta",
    //"rust-client.disableRustup": true,
    "editor.lineHeight": 13,
    "editor.rulers": [80, 120, 160],
    "workbench.colorCustomizations": {
        "editorRuler.foreground": "#eee"
    },
    "[markdown]": {
        "editor.wordWrap": "off",
    },
    "editor.fontSize": 11,
    "editor.suggestSelection": "first",
    "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
    "terminal.integrated.rendererType": "canvas",
    "workbench.colorTheme": "Default Light+",
    "files.associations": {
        "*.xml": "html"
    },
    "editor.accessibilitySupport": "off",
    "java.home": "C:\\Program Files\\Android\\Android Studio\\jre",
    "java.configuration.checkProjectSettingsExclusions": false,
    "rust-analyzer.checkOnSave.enable": false,  // bogus intellisense errors in problems pane
    "rust-analyzer.hoverActions.enable": false, // "goto XYZ" menu items when hovering over underlined identifiers
    "rust-analyzer.inlayHints.enable": false,   // inline hints like ": usize"
    "rust-analyzer.lens.enable": false,         // inline hints like "2 implementations"
    "rust-analyzer.rustfmt.overrideCommand": null,
    "editor.minimap.enabled": true,
    "rust-analyzer.updates.askBeforeDownload": false,
    "workbench.editor.openSideBySideDirection": "down",
    "editor.semanticTokenColorCustomizations": {
        "rules": {
            "keyword.unsafe": {
                "foreground": "#ff0000",
                "fontStyle": "bold",
            },
        }
    }

    //"editor.parameterHints.enabled": false,
    //"rust-analyzer.callInfo.full": false,
}
```
