# Forensics Git 1 — picoCTF 2026

Hints given:
1. How can you extract the directory from the disk image?

Download disk image:

    $ wget https://challenge-files.picoctf.net/c_plain_mesa/4538dd1f2e93e907c17f0b663c0e1fae2d7054a72b4ee36977f20cfbf3b0a01c/disk.img.gz
    $ gunzip disk.img.gz

Do the same as in the last challenge to fetch all files:

    mmls disk.img
    7z x disk.img -oextracted_files
    find extracted_files/
    7z x extracted_files/2.img -oall_files
    find all_files/

Find the flag. It is in an earlier commit:

    cd all_files/home/ctf-player/Code/secrets/.git/
    find -type f -exec grep -aH flag {} \;
    git log --oneline
    5fb8194 (HEAD -> master) Remove flag
    177789a Add flag
    git checkout 177789a
    cat flag.txt
    picoCTF{g17_r3m3mb3r5_********}
