# Autorev 1 — picoCTF 2026

The server responds with:
- A number
- A binary blob that appears to be an executable

Testing locally:
- Download the binary from the server
- Run it with the provided number as input
- The binary prints “Correct”
- Sending the number itself back to the server also returns “Correct”

This suggests the challenge is simply:
1. Read the number the server sends
2. Send the exact same number back
3. Repeat this for each round

Create a script that:
- Connects to the server
- Reads the number
- Sends the same number back
- Repeats this 20 times

After 20 successful responses, the server returns the flag.

Here is my script:
```python
from pwn import *
import re

HOST = "mysterious-sea.picoctf.net"
PORT = 56595

def recv_clean(r):
    try:
        return r.recvline().decode(errors="ignore").strip()
    except EOFError:
        return None

def main():
    r = remote(HOST, PORT)

    # Velkomst
    welcome = recv_clean(r)
    print(welcome)

    while True:
        # Les tallet
        num_line = recv_clean(r)
        if num_line is None:
            break
        num_line = num_line.strip()
        if not num_line:
            continue

        # Hvis linja ikke er et tall, er vi sannsynligvis ferdige (f.eks. flagg / sluttmelding)
        if not re.fullmatch(r"\d+", num_line):
            print("Ikke-tall, trolig sluttmelding:", num_line)
            # LES RESTEN TIL EOF – HER KAN FLAGGET LIGGE
            while True:
                extra = recv_clean(r)
                if extra is None:
                    break
                print("Extra:", extra)
            break

        number = int(num_line)
        print(f"Tall: {number}")

        # "Here's the next binary in bytes:"
        info = recv_clean(r)
        print(info)

        # Hex-linje (ignoreres)
        hex_line = recv_clean(r)

        # Send hemmeligheten = tallet
        r.sendline(str(number).encode())

        # Les respons fra serveren (Correct! / neste runde)
        resp = recv_clean(r)
        if resp is None:
            break
        print("Server:", resp)

    r.close()

if __name__ == "__main__":
    main()
```
