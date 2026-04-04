# Hidden Cipher 2 — picoCTF 2026

Inspect the ZIP file:

    file hiddencipher2.zip
    hiddencipher2.zip: Zip archive data, at least v2.0 to extract, compression method=deflate

Check for visible strings:

    strings hiddencipher2.zip | less

Extract the archive:

    unzip hiddencipher2.zip
    Archive:  hiddencipher2.zip
      inflating: hiddencipher2
     extracting: flag.txt

Check the extracted flag:

    cat flag.txt
    picoCTF{fake_flag}

Inspect the binary:

    file hiddencipher2
    hiddencipher2: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=599eedd164a0821201befcb967a2529efa0cc3ce, for GNU/Linux 3.2.0, not stripped

Connect to the remote service:

    nc crystal-peak.picoctf.net 62076
    What is 8 - 0? 8
    Encoded flag values:
    896, 840, 792, 888, 536, 672, 560, 984, 872, 416, 928, 832, 760, 784, 408, 832, 392, 880, 800, 760, 792, 392, 896, 832, 408, 912, 760, 408, 392, 448, 424, 808, 800, 400, 808, 1000

Running the program multiple times with different answers shows that the encoded values are a linear multiple of the answer.

Therefore, use a prompt where the answer equals 1:

    What is 5 - 4? 1
    112, 105, 99, 111, 67, 84, 70, 123, 109, 52, 116, 104, 95, 98, 51, 104, 49, 110, 100, 95, 99, 49, 112, 104, 51, 114, 95, 51, 49, 56, 53, 101, 100, 50, 101, 125

Convert these decimal values to ASCII using CyberChef:  
https://cyberchef.org/#recipe=From_Decimal('Space',false)&input=MTEyLCAxMDUsIDk5LCAxMTEsIDY3LCA4NCwgNzAsIDEyMywgMTA5LCA1MiwgMTE2LCAxMDQsIDk1LCA5OCwgNTEsIDEwNCwgNDksIDExMCwgMTAwLCA5NSwgOTksIDQ5LCAxMTIsIDEwNCwgNTEsIDExNCwgOTUsIDUxLCA0OSwgNTYsIDUzLCAxMDEsIDEwMCwgNTAsIDEwMSwgMTI1&oeol=FF

The decoded output reveals the real flag.
