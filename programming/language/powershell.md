# PowerShell

I hate powershell.
My main experience with it is scripting build servers for Windows.



## Pain points

### Execution Policy Nonsense

[Execution Policies](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.4)
are the kind of thing that make sense in theory... but then suddenly you're prevented from running `*.ps1` scripts without code signing or some
`PowerShell -ExecutionPolicy Bypass` nonsense, when you can run `*.cmd` or `*.bat` scripts without issue.
Possibly because the build scripts were served over a network share?

### Versioning and Breaking Changes

My VirtualBox management `*.ps1` scripts broke randomly on me... possibly after installing Visual Studio?
IIRC, it was some syntax changes when transitioning from PowerShell 1 → 2?
You can help avoid the issue with e.g. `PowerShell -Version 1.0` or similar nonsense... assuming all your target computers have a compatible version installed, which IME is a bad assumption.

### Return values and Outputs are Mingled

```powershell
function do_stuff() {
    Write-Output "Misc. Logging"
}

function f() {
    try {
        Write-Output "a"
        do_stuff
        return "b"
    } finally {
        Write-Output "c"
    }
}

$v = f
```

What's the value of `$v`?  That's right:

```
a
Misc. Logging
b
c
```

Every random function you call contributes to the "return" value.
You want a nice, clean, isolated value to be output from a function?
Resort to globals like it's the 1980s.
No I'm not kidding.
