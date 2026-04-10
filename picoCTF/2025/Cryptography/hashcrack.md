# hashcrack — picoCTF 2025

Hints:
1. Understanding hashes is very crucial.
2. Can you identify the hash algorithm? Look carefully at the length and structure of each hash.
3. Tried using any hash cracking tools?

I run the netcat command from the challenge.

The first hash: I find “password123” with a Google search (MD5).

The second hash: “letmein” (SHA‑1).

For the third hash I use:

    https://crackstation.net/

This gives “qwerty098” (SHA‑256).

I then get the flag.
