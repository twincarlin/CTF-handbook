# bytemancy 0 — picoCTF 2026

Send three bytes with value 0x65 (`'e'`) followed by a newline:

    printf '\x65\x65\x65\n' | nc candy-mountain.picoctf.net 54486

Alternative using socat (no newline needed):

    printf '\x65\x65\x65' | socat - TCP:candy-mountain.picoctf.net:54486
