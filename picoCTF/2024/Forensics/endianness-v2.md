# endianness-v2 — picoCTF 2024

CyberChef Magic helped here — it detected **Render Image** — but first:

    $ xxd -p challengefile

(You cannot combine `-e` with `-p` to swap endianness; `man xxd` confirms this.)

Paste the hex into CyberChef.

Apply:

- **Swap endianness (Hex, 4‑byte)**
- **From Hex**
- **Render Image**

You can tell it’s a JPEG (and that 4‑byte endianness is correct) from the signature:

    FF D8 FF E0   (JPEG header)
    → becomes
    E0 FF D8 FF   after swapping

Reference:  
https://en.wikipedia.org/wiki/List_of_file_signatures

The file could also have been loaded directly into CyberChef.

A writeup I found describes this alternative solution:

    $ hexdump -v -e '1/4 "%08x"' -e '"\n"' challengefile | xxd -r -p > challengefile.jpg
