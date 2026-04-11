# WinAntiDbg0x100 — picoCTF 2024

Downloaded the file on Windows and extracted it using the password.

Started debugging with x64dbg and selected the 32‑bit version (`x32dbg`).

    File > Open > `WinAntiDbg0x100.exe`
    View > Symbol Info > Look at the `.exe` > Search for `debug`

Double‑clicked the line containing `IsDebuggerPresent`.

Set a breakpoint.

    Debug > Run > Run (until the breakpoint)

Stepped over until reaching the line:

    00541604 | 74 15 | je winantidbg0x100.54161B

Double‑clicked and right‑clicked the line with memory location `54161B` > Set EIP Here.

Stepped over to a line slightly further down where the picoCTF flag appears.

Alternatively, after the breakpoint on the line with `IsDebuggerPresent`:

    Step into > Step into > Step over until `ret` > Change `EAX` from `1` to `0`

The picoCTF flag appears the same way.

Alternatively visible in the Log tab.
