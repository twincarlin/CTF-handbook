# Binary Digits — picoCTF 2026

The file appears to contain:
- Large amounts of padding (repeated 32‑bit sequences = 4 bytes)
- Actual binary data mixed in

Check file size:

    wc -m digits.bin
    70960

Factorizing:

    70960 = 2 * 2 * 2 * 2 * 5 * 887

This suggests the meaningful data is likely:

    70960 / 8 = 8870 bytes

Convert binary digits to bytes:

    tr -d '\n' < digits.bin | fold -w 8 | perl -lpe '$_=chr(oct("0b$_"))' > out.bin

Inspect the output:

    strings out.bin | head -1
    JFIF

This confirms the data is a JPEG file.

You can also verify this in CyberChef using “From Binary” with byte length 8:  
https://cyberchef.org/#recipe=From_Binary('Space',8)

A more direct conversion:

    tr -d '\n' < digits.bin | fold -w 8 | perl -ne 'chomp; print pack("B8", $_)' > out.jpg

Check again:

    strings out.jpg | head -1
    JFIF

Transfer the JPEG from Webshell to your local machine.

In Webshell:

    base64 out.jpg > out.jpg.b64
    cat out.jpg.b64

On Windows PC:

    certutil -decode BinaryDigits.jpg.b64 BinaryDigits.jpg

Open `BinaryDigits.jpg` to reveal the flag.
