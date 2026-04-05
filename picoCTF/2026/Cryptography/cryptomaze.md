# cryptomaze - picoCTF 2026

This challenge provides:
- the initial state of an LFSR  
- the tap positions  
- an AES‑encrypted flag in hex  

The goal is to reconstruct the AES‑128 key by reproducing the LFSR output and then decrypt the ciphertext.

Hints given:

- Hint 1: Use the LFSR’s initial state and taps to generate a 128‑bit sequence.  
- Hint 2: Convert this 128‑bit sequence into a 16‑byte AES key by grouping the bits into 8‑bit chunks and converting each chunk to a byte.  
- Hint 3: Decrypt the flag using AES in ECB mode.  
- Hint 4: Convert the encrypted flag from hex to bytes before decrypting.

I implemented the LFSR using the given initial state and taps, then generated the first 128 bits of its output.  
These 128 bits were split into sixteen 8‑bit chunks and converted into a 16‑byte AES‑128 key.  
Using this key, I decrypted the provided ciphertext (after converting it from hex to bytes) with AES‑ECB.  
The decrypted output revealed the flag.

Below are the scripts used for generating the key and decrypting the flag:

LFSR2AES.py:
```python
state = [0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 1, 0, 1, 1, 0, 0,
         1, 0, 0, 1, 0, 1, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1,
         0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 0, 1, 0, 1, 1,
         1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 1, 0, 1, 1]

taps = [63, 61, 60, 58]

output = []

for _ in range(128):
    # Output bit (LSB)
    output.append(state[-1])

    # Compute feedback bit
    new_bit = 0
    for t in taps:
        new_bit ^= state[t]

    # Shift right
    state = [new_bit] + state[:-1]

print("".join(str(b) for b in output))
```
convert.py:
```python
from Crypto.Cipher import AES

state = [0, 0, 1, 0, 0, 1, 0, 1, 1, 1, 1, 0, 1, 1, 0, 0,
         1, 0, 0, 1, 0, 1, 1, 0, 1, 0, 0, 1, 0, 1, 0, 1,
         0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 0, 1, 0, 1, 1,
         1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 1, 0, 1, 1]

taps = [63, 61, 60, 58]

bits = []

for _ in range(128):
    # Output MSB
    bits.append(state[0])

    # Compute feedback bit
    new_bit = 0
    for t in taps:
        new_bit ^= state[t]

    # Shift left
    state = state[1:] + [new_bit]

print("".join(str(b) for b in bits))

def bits_to_aes_key(bits):
    if len(bits) != 128:
        raise ValueError("Key must be exactly 128 bits")

    byte_values = []
    for i in range(0, 128, 8):
        byte = bits[i:i+8]
        value = int("".join(str(b) for b in byte), 2)
        byte_values.append(value)

    return bytes(byte_values)

aes_key = bits_to_aes_key(bits)
print("AES key (hex):", aes_key.hex())

ciphertext = bytes.fromhex("8f0e6d0f5b0dc1db201948b9e0cebd8fbe28e2f2cc42aa62c1a3ad408a0f0b3838338e7e04fbddef0c6260a4eb758417")
cipher = AES.new(aes_key, AES.MODE_ECB)
plaintext = cipher.decrypt(ciphertext)
print(plaintext)
```
