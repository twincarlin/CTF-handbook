# DISKO 4 — picoCTF 2026

Hints given:
1. How would you look for deleted files?

Examine disk image (there is no partition table, this is a FAT32 file system pure partition image):

    $ mmls disko-4.dd
    $ file -s disko-4.dd
    disko-4.dd: DOS/MBR boot sector, code offset 0x58+2, OEM-ID "mkfs.fat", Media descriptor 0xf8, sectors/track 32, heads 8, sectors 204800 (volumes > 32 MB), FAT (32 bit), sectors/FAT 1576, serial number 0x49838d0b, unlabeled

Find deleted files and extract the flag:

    $ fls -r -d disko-4.dd
    r/r * 522629:   log/messages
    r/r * 532021:   log/dont-delete.gz
    $ icat disko-4.dd 522629 > messages
    Error recovering deleted file (Invalid address in run (too large): 204800)
    $ icat disko-4.dd 532021 > dont-delete.gz
    $ gunzip dont-delete.gz
    $ cat dont-delete
    Here is your flag
    picoCTF{d3l_d0n7_h1d3_w3ll_********}
