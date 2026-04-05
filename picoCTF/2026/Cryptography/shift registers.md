# shift registers - picoCTF 2026

From chall.py I see the register size is 8 bits (mask 0xff), so there are only 255 possible seeds.

LFSR is an XOR‑based stream cipher.

Assume the plaintext starts with "picoCTF{".  
Then the first 8 keystream bytes can be recovered by:

    KS = CT XOR PT
    CT[0..7] = 21 c1 b7 05 76 4e 4b fd
    PT[0..7] = 70 69 63 6f 43 54 46 7b

So:

    KS = 51 A8 D4 6A 35 1A 0D 86

These bytes correspond directly to the first LFSR states.

Since an 8‑bit LFSR has only 255 possible seeds, we can brute‑force all seeds
and find which one produces this keystream prefix.

I do this with findSeed.py and get two possible seeds: 162 and 163.

Use these seeds to decrypt the full ciphertext with decrypt.py.

findSeed.py:
```python
ks_target = [0x51, 0xA8, 0xD4, 0x6A, 0x35, 0x1A, 0x0D, 0x86]

def steplfsr(lfsr):
    b7 = (lfsr >> 7) & 1
    b5 = (lfsr >> 5) & 1
    b4 = (lfsr >> 4) & 1
    b3 = (lfsr >> 3) & 1
    feedback = b7 ^ b5 ^ b4 ^ b3
    return ((feedback << 7) | (lfsr >> 1)) & 0xFF

for seed in range(256):
    l = seed
    ok = True
    for i in range(8):
        l = steplfsr(l)
        if l != ks_target[i]:
            ok = False
            break
    if ok:
        print("Mulig seed:", seed)
```
decrypt.py
```python
from Crypto.Util.number import long_to_bytes

ct_hex = "21c1b705764e4bfdafd01e0bfdbc38d5eadf92991cdd347064e37444e517d661cea9"
ct = bytes.fromhex(ct_hex)

def steplfsr(lfsr):
    b7 = (lfsr >> 7) & 1
    b5 = (lfsr >> 5) & 1
    b4 = (lfsr >> 4) & 1
    b3 = (lfsr >> 3) & 1
    feedback = b7 ^ b5 ^ b4 ^ b3
    return ((feedback << 7) | (lfsr >> 1)) & 0xFF

def decrypt_with_seed(seed, ct_bytes):
    lfsr = seed & 0xFF
    out = bytearray()
    for c in ct_bytes:
        lfsr = steplfsr(lfsr)
        ks = lfsr
        out.append(c ^ ks)
    return bytes(out)

for seed in (162, 163):
    pt = decrypt_with_seed(seed, ct)
    print("Seed:", seed)
    print(pt)
    print(pt.decode(errors="ignore"))
```
