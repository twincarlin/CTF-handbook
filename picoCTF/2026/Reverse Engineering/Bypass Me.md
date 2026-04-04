# Bypass Me — picoCTF 2026

Connect to the challenge server:

    ssh ctf-player@foggy-cliff.picoctf.net -p61906

Inspect the binary:

    file bypassme.bin
    bypassme.bin: setuid ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=b8ad33d01afb381f4e12fa6e6353f7eaec4af3a2, for GNU/Linux 3.2.0, with debug_info, not stripped

Search for interesting strings:

    strings bypassme.bin | more
    strings bypassme.bin | grep flag
    ../../root/flag.txt
    Error reading flag.
    _flags
    flag_file
    flag
    _flags2

Check for hints:

    strings bypassme.bin | grep Hint
    Hint: Input must match something special...

Testing input shows that special characters are sanitized:

    Raw Input:      [|+\§!"#¤%&/()=?`@£${[]}´¨^~'*-_.:,;]
    Sanitized Input:[]
    Hint: Input must match something special...
    Access Denied

Numbers and symbols are also removed:

    Raw Input:      [1234567890<>øåæ]
    Sanitized Input:[]

Alphabetic characters remain:

    Raw Input:      [qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM]
    Sanitized Input:[qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM]

Check shell profile for clues:

    cat .profile
    export PS1="\[\e[35m\]\u\[\e[m\]@\[\e[35m\]pico-chall\[\e[m\]$ "
    
Testing this as input:

    Raw Input:      [\[\e[35m\]\u\[\e[m\]@\[\e[35m\]pico-chall\[\e[m\]$ ]
    Sanitized Input:[emuemempicochallem]
    Hint: Input must match something special...
    Access Denied

Since no debugger is available on the server, copy the binary locally:

    scp -P63932 ctf-player@foggy-cliff.picoctf.net:/home/ctf-player/bypassme.bin .

Open in gdb and inspect functions:

    (gdb) info functions
    File /home/ctf-player/bypassme.c:
    34:     void auth_sequence();
    16:     void decode_password(char*);
    45:     void intro_sequence();
    73:     int main();
    24:     void sanitize(char const*, char*);
    7:      void type_out(char const*, unsigned int);

Disassemble the password decoder:

    (gdb) disass decode_password
       0x0000000000001352 <+31>:    movabs $0xc9cff9d8cfdadff9,%rax
       0x0000000000001360 <+45>:    movw   $0xd8df,-0xb(%rbp)
       0x0000000000001366 <+51>:    movb   $0xcf,-0x9(%rbp)
       0x0000000000001386 <+83>:    xor    $0xffffffaa,%eax

The encoded bytes are:

    f9 df da cf d8 f9 cf c9 df d8 cf

These are XORed with 0xAA.

Decode using CyberChef (From Hex → XOR 0xAA):  
https://cyberchef.org/#recipe=From_Hex('Auto')XOR({'option':'Hex','string':'aa'},'Standard',false)&input=ZjkgZGYgZGEgY2YgZDggZjkgY2YgYzkgZGYgZDggY2Y&oeol=FF

The result is:

    SuperSecure

This is the required password.
