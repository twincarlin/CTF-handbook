# Timeline 1 — picoCTF 2026

Hints given:
1. Create a Sleuthkit MAC timeline!
2. Look at recent timestamps
3. Pay close attention to timestamps near an anti-forensic action
4. Filter only new files by grepping for macb
   
I cannot solve this in the Webshell, so I do it on WSL on Windows, and with Sleuthkit like the last challenge.

Download and unpack image file:

    $ wget https://challenge-files.picoctf.net/c_plain_mesa/fef9e3937fced503da228c6affaea69ed51d6234ed8fde14a52b573777b869e7/partition4.img.gz
    $ gunzip partition4.img.gz

Extract metadata from image and make MAC timeline:

    $ fls -m / -r partition4.img > body.txt
    $ mactime -b body.txt > timeline.txt

Find the file with old timestamp:

    $ head -1 timeline.txt
    Tue Jan 01 1985 18:00:00       41 macb r/rrw-r--r-- 0        0        4945     /bin/bcab

Used the hints and found an interesting chat file:

    $ grep macb timeline.txt
    $ icat partition4.img 32716 > chat
    $ cat chat
    NTczNDE3aDEzcl83aDRuXzdoM18xNDU3XzU4NTI3YmIyMjIK
    $ base64 -d < chat
    573417h13r_7h4n_7h3_1457_58527bb222

Can also be converted in Cyberchef (From Base64), recognize this as the part of the flag inside curly brackets.
