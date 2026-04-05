# MY GIT — picoCTF 2026

Clone the challenge repository:

    git clone ssh://git@foggy-cliff.picoctf.net:58865/git/challenge.git

Read the instructions:

    cat challenge/README.md
    Only flag.txt pushed by root:root@picoctf will be updated with the flag.

Enter the repo:

    cd challenge/

Set Git author and committer identity to match the required `root:root@picoctf`:

    GIT_AUTHOR_NAME="root" \
    GIT_AUTHOR_EMAIL="root@picoctf" \
    GIT_COMMITTER_NAME="root" \
    GIT_COMMITTER_EMAIL="root@picoctf"

Create the file:

    touch flag.txt

Add it:

    git add flag.txt

Commit it:

    git commit -m "Add flag"

Push it, and the server replaces the file with the real flag.
