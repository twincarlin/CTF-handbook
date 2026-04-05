# StegoRSA — picoCTF 2026

- Hint 1: Metadata can tell you more than you expect.
- Hint 2: Hex can be turned back into a key file.

```
$ exiftool image.jpg | grep Comment
Comment : 2d2d2d2d2d424547494e2050......04b45592d2d2d2d2d0a
```

This is a hex‑encoded private key in PEM format.
The beginning decodes to: -----BEGIN PRIVATE KEY-----

Create key.pem using a small script (createPEM.py) that reads the hex from the image and converts it back to the PEM key.

Decrypt the encrypted flag with:

    openssl rsautl -decrypt -inkey key.pem -in flag.enc

Here is my createPEM.py:

```python
import binascii

hexdata = "2d2d2d2d2d424547494e2050......45592d2d2d2d2d0a"  # hele strengen her
pem = binascii.unhexlify(hexdata)

with open("key.pem", "wb") as f:
    f.write(pem)
```
