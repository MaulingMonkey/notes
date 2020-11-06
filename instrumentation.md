# Event Tracing for Windows

* [github.com/microsoft/rust_win_etw](https://github.com/microsoft/rust_win_etw) ([/r/rust](https://www.reddit.com/r/rust/comments/go3g1n/new_crate_rust_support_for_event_tracing_for/) post)
* [github.com/flowerinthenight/rustttrace](https://github.com/flowerinthenight/rusttrace)
* CLI tools:
  [logman](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc753820(v=ws.11)?redirectedfrom=MSDN)
  [xperf](https://docs.microsoft.com/en-us/windows-hardware/test/wpt/xperf-command-line-reference)
  [wevtutil](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/wevtutil)
* [Instrumenting Your Code with ETW](https://docs.microsoft.com/en-us/windows-hardware/test/weg/instrumenting-your-code-with-etw)

```cmd
:: provider names -> guids
C:\local\etw>logman query providers
...
Microsoft-Windows-NDIS                   {CDEAD503-17F5-4A3E-B7AE-DF8CC2902EB9}
Microsoft-Windows-NDIS-PacketCapture     {2ED6006E-4729-4609-B423-3EE7BCD678EF}
Microsoft-Windows-NdisImPlatformEventProvider {11C5D8AD-756A-42C2-8087-EB1B4A72A846}
...
```

http://noahdavids.org/self_published/native_windows_packet_capture.html
```cmd
C:\local\etw>netsh trace start capture=yes traceFile=C:\local\etw\trace
...
C:\local\etw>netsh trace stop
Merging traces ... done
Generating data collection ... done
The trace file and additional troubleshooting information have been compiled as "C:\local\etw\trace.cab".
File location = C:\local\etw\trace
Tracing session was successfully stopped.
...

C:\local\etw>expand trace.cab -F:report.etl .
...

report.etl = trace
trace.cab = compressed trace / archive format?
```

# Xperf

* [Random ASCII Category Archives: xperf](https://randomascii.wordpress.com/category/xperf/)
* [Profiling with Xperf](https://developer.mozilla.org/en-US/docs/Mozilla/Performance/Profiling_with_Xperf) (developer.mozilla.org)
    * [Xperf Wait Analysis - Finding Idle Time](https://randomascii.wordpress.com/2012/05/05/xperf-wait-analysisfinding-idle-time/)
    * [Summarizing Xperf CPU Usage with Flame Graphs](https://randomascii.wordpress.com/2013/03/26/summarizing-xperf-cpu-usage-with-flame-graphs/)

# Misc

* [rust: tracers](https://github.com/anelson/tracers)
