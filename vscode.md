For making custom extensions with commands:
*   https://marketplace.visualstudio.com/items?itemName=ryuta46.multi-command
*   https://github.com/ryuta46/vscode-multi-command
*   https://github.com/ryuta46/vscode-multi-command/blob/master/src/extension.ts

# Style

* [Adding italics support to your favourite VSCode theme](https://dev.to/salted-bytes/adding-italics-support-to-your-favourite-vscode-theme-2ec9) (dev.to)
* [Color Themes](https://code.visualstudio.com/docs/getstarted/themes) (code.visualstudio.com)

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
