# Secure Password Database — picoCTF 2026

Hints given:
1. How does the hashing algorithm work?

Download the program:

    $ wget https://challenge-files.picoctf.net/c_candy_mountain/4724bf8a88a0b49eea63b1458e128d84e7f1322a8327751b9608fa059f53fbd1/system.out

Examine the program locally with `gdb`, and especially the `hash` method (set breakpoint, finish and print the registry to find the return value in `rax`):

    $ gdb ./system.out 
    (gdb) info functions hash
    All functions matching regular expression "hash":
    
    Non-debugging symbols:
    0x0000000000001309  hash
    (gdb) break hash
    Breakpoint 1 at 0x1311
    (gdb) run
    (gdb) finish
    Run till exit from #0  0x0000566567457311 in hash ()
    0x00005665674573ce in make_secret ()
    (gdb) info registers rax
    rax            0xd3770d6251b31be2  -3209081493549540382

Run the program locally with a dummy flag file. Use the same password and hash from running `gdb` and extracting `rax`:

    $ strings system.out | grep "^flag"
    flag.txt
    $ cat > flag.txt
    picoCTF{dummy}
    $ ./system.out
    Enter your hash to access your account!
    -3209081493549540382
    picoCTF{dummy}

Do the same remotely to get the real flag.
