# WinAntiDbg0x200 — picoCTF 2024

Did the same steps as in WinAntiDbg0x100.

Set a breakpoint on the line before `IsDebuggerPresent`.

Changed `EAX` to `0`.

Stepped over.

Right‑clicked two lines above > Set EIP Here.

Stepped over until the picoCTF flag appeared.
