# weirdSnake — picoCTF 2024

The code builds a list called `input_list`.

Used Visual Studio Code to strip away `.*\(` before the elements and `\)\n` after the elements.

The list becomes:

    4 54 41 0 112 32 25 49 33 3 0 0 57 32 108 23 48 4 9 70 7 110 36 8 108 7 49 10 4 86 43 105 114 91 0 71 106 124 93 78

A `key_str` is created from the characters `J _ o 3 t`.

The length is compared and `key_list` is extended in a loop.

Finally, XOR is applied between `input_list` and `key_list` to produce the flag.

Used CyberChef: **From Decimal**, **XOR key `J_o3t`** (UTF‑8), but this did not give a flag in the correct format.

Because of XOR properties, instead tried: **From Decimal**, **XOR key `picoCTF{`** (UTF‑8), and then saw that the correct key must be `t_Jo3`.

The flag is found with: **From Decimal**, **XOR key `t_Jo3`** (UTF‑8)

CyberChef recipe:  
https://gchq.github.io/CyberChef/#recipe=From_Decimal('Space',false)XOR(%7B'option':'UTF8','string':'picoCTF%7BN0t_sO_'%7D,'Standard',false/disabled)XOR(%7B'option':'UTF8','string':'t_Jo3'%7D,'Standard',false)XOR(%7B'option':'UTF8','string':'t_Jo3'%7D,'Standard',false/disabled)To_Decimal('Space',false/disabled)&input=NCA1NCA0MSAwIDExMiAzMiAyNSA0OSAzMyAzIDAgMCA1NyAzMiAxMDggMjMgNDggNCA5IDcwIDcgMTEwIDM2IDggMTA4IDcgNDkgMTAgNCA4NiA0MyAxMDUgMTE0IDkxIDAgNzEgMTA2IDEyNCA5MyA3OA&ieol=CRLF

Python compiler (to bytecode):  
https://godbolt.org/noscript/python

Can be compared with someone who has reverse engineered the code:  
https://sudorem.dev/posts/pico24-weirdsnake/
