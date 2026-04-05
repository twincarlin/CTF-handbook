# Printer Shares — picoCTF 2026

List available SMB shares:

    smbclient -L //mysterious-sea.picoctf.net -p 57873 -N

Connect to the `shares` share:

    smbclient //mysterious-sea.picoctf.net/shares -p 57873 -N

List files:

    smb: \> ls

Download the flag:

    smb: \> get flag.txt
    smb: \> quit

Read it locally:

    cat flag.txt
