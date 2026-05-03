# Forensics Git 0 — picoCTF 2026

Hints given:
1. How can you extract the directory from the disk image?

Download disk image:

    $ wget https://challenge-files.picoctf.net/c_plain_mesa/96db2eea3d6d3e215d3dc2289457a1bc10b17b1de69c46996a171f4f689db74b/disk.img.gz
    $ gunzip disk.img.gz

Check partition table (Slot 002 and 004 seems relevant):

    $ mmls disk.img
    DOS Partition Table
    Offset Sector: 0
    Units are in 512-byte sectors
    
          Slot      Start        End          Length       Description
    000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
    001:  -------   0000000000   0000002047   0000002048   Unallocated
    002:  000:000   0000002048   0000616447   0000614400   Linux (0x83)
    003:  000:001   0000616448   0001140735   0000524288   Linux Swap / Solaris x86 (0x82)
    004:  000:002   0001140736   0002097151   0000956416   Linux (0x83)

Here are the files:

    $ fls -o 1140736 disk.img
    d/d 64770:      home
    d/d 11: lost+found
    d/d 64769:      boot
    d/d 32385:      etc
    d/d 13: proc
    d/d 64772:      dev
    d/d 14: tmp
    d/d 15: lib
    d/d 64773:      var
    d/d 22: bin
    d/d 24: sbin
    d/d 64778:      usr
    d/d 64948:      media
    d/d 170:        mnt
    d/d 171:        opt
    d/d 172:        root
    d/d 173:        run
    d/d 64952:      srv
    d/d 174:        sys
    d/d 65275:      swap
    V/V 119417:     $OrphanFiles

Try to extract everything:

    $ tsk_recover -o 1140736 -d 64770 disk.img unpacked_home/
    Files Recovered: 0

Check content of /home (seems relevant):

    $ fls -o 1140736 disk.img 64770
    d/d 64771:      ctf-player

Probing:

    $ icat disk.img 65692 > note.txt
    Cannot determine file system type
    $ icat -o 1140736 disk.img 65692 > note.txt
    $ cat note.txt
    The picoCTF flag format is 'picoCTF{}' where there is some leetspeak phrase in between the curly braces

Extract everything with 7zip

    $ sudo apt install p7zip-full
    $ 7z x disk.img -oextracted_files
    $ find extracted_files/
    extracted_files/
    extracted_files/1.img
    extracted_files/0.img
    extracted_files/2.img
    $ 7z x extracted_files/2.img -oall_files
    $ cd all_files/home/
    $ find -type f

Search for the flag, and found it:

    $ find -type f -exec cat {} \; | grep flag
    The picoCTF flag format is 'picoCTF{}' where there is some leetspeak phrase in between the curly braces
    grep: (standard input): binary file matches
    $ find -type f -exec cat {} \; | grep -a flag | grep flag
    The picoCTF flag format is 'picoCTF{}' where there is some leetspeak phrase in between the curly braces
    0000000000000000000000000000000000000000 327681bb38cf467cec328eec9707b240e3e74ced ctf-player <ctf-player@example.com> 1763542167 +0000  commit (initial): Wrap this phrase in the flag format: g17_1n_7h3_d15k_041217d8
    $ find -type f -exec cat {} \; | grep -a flag | grep flag | grep _ | cut -d":" -f2,3
     Wrap this phrase in the flag format: g17_1n_7h3_d15k_041217d8
