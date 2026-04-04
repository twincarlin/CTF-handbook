# Hidden Cipher 1 — picoCTF 2026

Check the ZIP file:

    file hiddencipher.zip
    hiddencipher.zip: Zip archive data, at least v2.0 to extract, compression method=deflate

Search for any obvious flags:

    strings hiddencipher.zip | grep pico
    picoCTF{fake_flag}PK

Extract the archive:

    unzip hiddencipher.zip
    Archive:  hiddencipher.zip
      inflating: hiddencipher
     extracting: flag.txt

Check the extracted flag:

    cat flag.txt
    picoCTF{fake_flag}

Inspect the binary:

    file hiddencipher
    hiddencipher: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), statically linked, no section header

A hint suggests the binary is packed with UPX:

    strings hiddencipher | grep UPX | grep -v "^UPX"
    $Info: This file is packed with the UPX executable packer http://upx.sf.net $
    $Id: UPX 4.24 Copyright (C) 1996-2024 the UPX Team. All Rights Reserved. $

Check UPX availability:

    which upx
    /usr/bin/upx

Try unpacking:

    upx -d hiddencipher
    upx: hiddencipher: CantUnpackException: need a newer version of UPX
    Unpacked 0 files.

Run the remote service:

    nc candy-mountain.picoctf.net 52550
    Here your encrypted flag:
    235a201d702015483b1d412b265d3313501f0c072d135f0d2002302d01156a57224306172e

Run the local binary with a test input:

    ./hiddencipher 235a201d70201548251358110c552f135409
    Here your encrypted flag:
    235a201d70201548251358110c552f135409

Hint 3 says: Think XOR. XORing twice with the same key restores the original.

This means:
- The local encrypted output = XOR(flag_local, key)
- The remote encrypted output = XOR(flag_real, key)
- Therefore: flag_real = XOR(remote_encrypted, key)

To get the key, XOR the known local plaintext (`picoCTF{fake_flag}`) with the local encrypted output.

Then use that key to XOR the remote encrypted flag.

Example CyberChef usage:

    Input: S3Cr3tS3Cr3tS3Cr3tS3Cr3tS3Cr3tS3Cr3tS
    XOR key (hex): 235a201d702015483b1d412b265d3313501f0c072d135f0d2002302d01156a57224306172e
    
https://cyberchef.org/#recipe=XOR({'option':'Hex','string':'235a201d702015483b1d412b265d3313501f0c072d135f0d2002302d01156a57224306172e'},'Standard',false)&input=UzNDcjN0UzNDcjN0UzNDcjN0UzNDcjN0UzNDcjN0UzNDcjN0Uw&oeol=FF

The decrypted output reveals the real flag.
