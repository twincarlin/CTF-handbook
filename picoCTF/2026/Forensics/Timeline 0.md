# Timeline 0 — picoCTF 2026

Hints given:
1. Create a Sleuthkit MAC timeline!
2. Sloppy timestomping can yield strange (very old) timestamps

I cannot solve this in the Webshell, so I do it on WSL on Windows.

Install Sleuthkit and download image file:

    $ sudo apt install sleuthkit
    $ wget https://challenge-files.picoctf.net/c_plain_mesa/aa1f8ba93409887e081435732d7037c45b30a8442853bf07c9e84fe4d0e0bc19/partition4.img.gz
    $ gunzip partition4.img.gz

Try to read partition table (no result): 

    $ mmls partition4.img

Extract metadata from image and make MAC timeline:

    $ fls -m / -r partition4.img > body.txt
    $ mactime -b body.txt > timeline.txt

Find the file with old timestamp:

    $ head -1 timeline.txt
    Tue Jan 01 1985 18:00:00       41 macb r/rrw-r--r-- 0        0        4945     /bin/bcab

Find the inode number and extract the file:

    $ fls -r partition4.img | grep bcab
    $ icat partition4.img 4945 > bcab
    $ cat bcab
    NzFtMzExbjNfMHU3MTEzcl9oM3JfNDNhMmU3YWYK

Convert this in Cyberchef (From Base64), recognize this as the part of the flag inside curly brackets.
