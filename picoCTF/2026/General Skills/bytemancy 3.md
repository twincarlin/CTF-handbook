# bytemancy 3 — picoCTF 2026

Hints given:

- Hint 1: `objdump -t spellbook` reveals the symbol table.
- Hint 2: Send the addresses as **4 raw bytes** in **little‑endian** order.
- Hint 3: `pwnlib.util.packing.p32()` helps build payloads.

First, check which functions the server will ask for:

    grep -A4 "SPELLBOOK_FUNCTIONS =" app.py
    SPELLBOOK_FUNCTIONS = [
        "ember_sigil",
        "glyph_conflux",
        "astral_spark",
        "binding_word",

Next, extract the function addresses from the binary:

    objdump -t spellbook | grep "ember_sigil\\|glyph_conflux\\|astral_spark\\|binding_word"
    08049176 g     F .text  00000024              ember_sigil
    0804919a g     F .text  00000027              glyph_conflux
    080491e3 g     F .text  00000031              binding_word
    080491c1 g     F .text  00000022              astral_spark

Convert each address to **little‑endian raw bytes** (Hint 2):

- 0x08049176 → `\x76\x91\x04\x08`
- 0x0804919a → `\x9a\x91\x04\x08`
- 0x080491e3 → `\xe3\x91\x04\x08`
- 0x080491c1 → `\xc1\x91\x04\x08`

Create a script that maps spell names to their corresponding byte sequences:

    cat interactive.py | grep -A4 "answers ="
    answers = {
        b"ember_sigil":     b"\x76\x91\x04\x08",
        b"glyph_conflux":   b"\x9a\x91\x04\x08",
        b"binding_word":    b"\xe3\x91\x04\x08",
        b"astral_spark":    b"\xc1\x91\x04\x08",
    }

Run `interactive.py` and it automatically responds with the correct 4‑byte payload for each spell the server asks for.

This completes the challenge and prints the flag.

Here is my python-script `interactive.py`
```python
#!/usr/bin/env python3
from pwn import *

# Server host/port
HOST = "green-hill.picoctf.net"
PORT = 55387

# Mapping of function names: raw 4-byte little-endian addresses
answers = {
    b"ember_sigil":     b"\x76\x91\x04\x08",
    b"glyph_conflux":   b"\x9a\x91\x04\x08",
    b"binding_word":    b"\xe3\x91\x04\x08",
    b"astral_spark":    b"\xc1\x91\x04\x08",
}

# Connect
io = remote(HOST, PORT)

while True:
    try:
        line = io.recvline()
    except EOFError:
        break
    if not line:
        break
    print(line.decode().rstrip())

    # Check which function name appears in the line
    for name, raw in answers.items():
        if name in line:
            print(f"Sending bytes for {name.decode()}")
            io.send(raw)
            break

io.interactive()
```
