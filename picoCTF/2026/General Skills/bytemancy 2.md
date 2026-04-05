# bytemancy 2 — picoCTF 2026

Using Python `print()` does **not** work for raw bytes.  
It encodes strings as UTF‑8, which transforms `\xff` into `0xc3 0xbf`.

Example proving this:

    python3 -c "print('\xff\xff\xff\n')" | hexdump -C
    00000000  c3 bf c3 bf c3 bf 0a 0a

But `printf` sends **raw bytes**:

    printf '\xff\xff\xff\n' | hexdump -C
    00000000  ff ff ff 0a

So the simplest working solution is:

    printf '\xff\xff\xff\n' | nc lonely-island.picoctf.net 58598

Binary‑safe Python alternative:

    python3 -c "import sys; sys.stdout.buffer.write(b'\xff\xff\xff\n')" \
        | nc lonely-island.picoctf.net 58598

Or using socat:

    python3 -c "import sys; sys.stdout.buffer.write(b'\xff\xff\xff')" \
        | socat - TCP:lonely-island.picoctf.net:58598

Or using pwntools:

    cat byte.py
```python
from pwn import *
r = remote("lonely-island.picoctf.net", 49304)
r.send(b"\xff\xff\xff")
r.interactive()
```
