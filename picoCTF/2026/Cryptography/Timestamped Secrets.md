# Timestamped Secrets - picoCTF 2026

The challenge states:
Someone encrypted a message using AES in ECB mode
and used the timestamp as the key.

Also I know that AES is a symmetric block cipher, which means that encryption key is the same as the decryption key

We get a new hint in message.txt:

    $ cat message.txt
    Hint: The encryption was done around 1770242606 UTC
    Ciphertext (hex): 24162f53d9b29255e635230b821cb8baca14461d54b2955401a049477e201fe9

Check the timestamp 1770242606 here:  
https://www.epochconverter.com/

    Your time zone: Wednesday 4 February 2026 23:03:26 GMT+01:00

We also have the encryption script, so we can write a decryption script
that brute‑forces the key by trying timestamps around the given value
and checking for output containing “picoCTF”.

Created decrypt.py to do this:

```python
from hashlib import sha256
from Crypto.Cipher import AES
import datetime

ct = bytes.fromhex("24162f53d9b29255e635230b821cb8baca14461d54b2955401a049477e201fe9")

def derive_key(ts):
    return sha256(str(ts).encode()).digest()[:16]

def decrypt_with_ts(ts):
    key = derive_key(ts)
    cipher = AES.new(key, AES.MODE_ECB)
    return cipher.decrypt(ct)

# søk over 14 dager rundt 7. februar 2026
start = int(datetime.datetime(2026, 2, 1).timestamp())
end   = int(datetime.datetime(2026, 2, 14).timestamp())

for ts in range(start, end):
    pt = decrypt_with_ts(ts)
    if b"picoCTF" in pt:
        print("FUNNET:", ts, pt)
        break
```
