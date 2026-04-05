# bytemancy 1 — picoCTF 2026

The challenge requires sending **1751 bytes of value 0x65** (`'e'`).

One way is to generate the bytes interactively:

    python3
    >>> b"\x65" * 1751
    Ctrl-D
    nc foggy-cliff.picoctf.net 55861
    (paste the Python output)

Or do it in one command:

    python3 -c "print('\x65' * 1751)" | nc foggy-cliff.picoctf.net 59625

Using socat:

    printf '\x65%.0s' $(seq 1 1751) | socat - TCP:foggy-cliff.picoctf.net:52770

Or a fully binary‑safe Python version:

    python3 -c "import sys; sys.stdout.buffer.write(b'\x65'*1751)" | socat - TCP:foggy-cliff.picoctf.net:52770
