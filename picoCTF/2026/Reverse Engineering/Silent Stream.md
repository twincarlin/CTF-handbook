# Silent Stream — picoCTF 2026

Download the files:

    wget https://challenge-files.picoctf.net/c_plain_mesa/bae6949c00801bf36fe2420d870d1d3319b93be7f7eb0325d34d7afd3f3e647c/packets.pcap
    wget https://challenge-files.picoctf.net/c_plain_mesa/bae6949c00801bf36fe2420d870d1d3319b93be7f7eb0325d34d7afd3f3e647c/encrypt.py

From `encrypt.py`, the encryption is:

    encrypted_byte = (byte + 42) mod 256

Try inspecting the pcap:

    tshark -r packets.pcap

No visible data appears, so extract raw TCP stream:

    tshark -r packets.pcap -z follow,tcp,raw,0 > out.bin

Copy the extracted hex data into a file:

    cat > data.txt

The data appears padded, similar to JPEG structure.  
Subtracting 42 from the first bytes gives:

    FF D8 FF E0 00 10 32 2E 31 2E 00 01 01 00 00 01 00 01 00 00

`FF D8 FF E0` is a JPEG header, and `00 10 32 2E 31 2E` is JFIF/EXIF metadata.

Write a Python script that:

- Reads the hex lines from the file  
- Removes line breaks  
- Converts hex to bytes  
- Subtracts 42 from each byte (mod 256)  
- Writes the result to a JPEG file  

Here is my script:

```python
# Les inn hex-data fra fil
with open("data.txt", "r") as f:
    hexdata = f.read().replace("\n", "").strip()

# Konverter hex → bytes
data = bytes.fromhex(hexdata)

# Subtraher 42 fra hver byte (mod 256)
decoded = bytes((b - 42) % 256 for b in data)

# Skriv resultatet til en JPEG-fil
with open("output.jpg", "wb") as f:
    f.write(decoded)

print("Ferdig! Lagret som output.jpg")
```

Transfer the resulting JPEG from Webshell to your PC.

In Webshell:

    base64 output.jpg > out.b64
    cat out.b64

On Windows CMD:

    certutil -decode out.b64 output.jpg

Open `output.jpg` to reveal the flag.
