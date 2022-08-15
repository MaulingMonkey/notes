## Overview
*   [Debugger](https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/_debugger/)

## debugapi.h

Basic C API for limited debugging.

*   [Writing the Debugger's Main Loop](https://docs.microsoft.com/en-us/windows/win32/debug/writing-the-debugger-s-main-loop)
    *   [DebugActiveProcess](https://docs.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-debugactiveprocess)\[[Stop](https://docs.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-debugactiveprocessstop)\]
    *   [IsDebuggerPresent](https://docs.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-isdebuggerpresent) / [CheckRemoteDebuggerPresent](https://docs.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-checkremotedebuggerpresent)
    *   [WaitForDebugEvent](https://docs.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-waitfordebugevent)\[[Ex](https://docs.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-waitfordebugeventex)\]
    *   [ContinueDebugEvent](https://docs.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-continuedebugevent)

## dbgeng.h

A more complicated COM-filled API rich with breakpoints etc. (behind WinDbg, VS?)

*   Initial factory methods
    *   [DebugCreate](https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/dbgeng/nf-dbgeng-debugcreate)\[[Ex](https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/dbgeng/nf-dbgeng-debugcreateex)\] - local debugging
    *   [DebugConnect](https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/dbgeng/nf-dbgeng-debugconnect)\[[Wide](https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/dbgeng/nf-dbgeng-debugconnectwide)\] - remote machine debugging
    *   [Client Objects](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/client-objects)
*   [dbgeng.h header](https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/dbgeng/)
*   [Debug Engine Interfaces](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/client-com-interfaces)
*   [Writing DbgEng Extension Code](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/writing-dbgeng-extension-code)

#### Files of Note
*   `dbgeng.h`
*   `dbgeng.lib`

## Additional References

*   [Creating Guard Pages](https://docs.microsoft.com/en-us/windows/win32/memory/creating-guard-pages)
*   [VirtualAllocEx](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualallocex) (to allocate memory in a remote process)
*   [DebugBreakProcess](https://docs.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-debugbreakprocess) (to generate a breakpoint in a remote process)
