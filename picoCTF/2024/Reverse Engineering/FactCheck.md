# FactCheck — picoCTF 2024

Extract the visible part of the flag:

    $ strings bin | grep pico
    picoCTF{wELF_d0N3_mate_

Upload the binary to an online decompiler:
https://dogbolt.org/

Used the Ghidra output.

Copied the decompiled code into Visual Studio Code and renamed variables to make the logic readable.

    local_xx → char_y
