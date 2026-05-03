# Forensics Git 2 — picoCTF 2026

Hints given:
1. We think the deletion was interrupted before any git objects were touched
2. From the description: The agents interrupted the perpetrator's disk deletion routine. Can you recover this git repo?

Do the same as in the last challenge to fetch all files:

    mmls disk.img
    7z x disk.img -oextracted_files
    find extracted_files/
    7z x extracted_files/2.img -oall_files
    find all_files/

Examine the local git repo. No results:

    $ cd all_files/home/ctf-player/Code/killer-chat-app/
    $ find -type f -exec grep -aH flag {} \;
    $ git log --oneline
    fatal: your current branch 'master' does not have any commits yet
    $ git reset --hard HEAD
    fatal: ambiguous argument 'HEAD': unknown revision or path not in the working tree.
    Use '--' to separate paths from revisions, like this:
    'git <command> [<revision>...] -- [<file>...]'
    $ git ls-tree -r HEAD
    fatal: Not a valid object name HEAD
    $ git checkout -f main
    error: pathspec 'main' did not match any file(s) known to git
    $ git checkout -f master
    error: pathspec 'master' did not match any file(s) known to git
    $ git checkout .
    Updated 5 paths from the index
    $ find -type f -exec grep -aH flag {} \;
    $ less logs/
    1.txt  2.txt  4.txt
    $ git log --all --oneline --graph
    $ git fsck --full --unreachable --no-reflogs
    Checking object directories: 100% (256/256), done.
    notice: HEAD points to an unborn branch (master)
    notice: No default references
    dangling commit 01533f718556a0e59f1467dae4fa462eed82c2a1
    $ git show 01533f7
    commit 01533f718556a0e59f1467dae4fa462eed82c2a1
    Author: ctf-player <ctf-player@example.com>
    Date:   Wed Nov 19 10:47:20 2025 +0000
    
        Add random chat log
    
    diff --git a/logs/4.txt b/logs/4.txt
    new file mode 100644
    index 0000000..6627387
    --- /dev/null
    +++ b/logs/4.txt
    @@ -0,0 +1,3 @@
    +Pip: My cat thinks the keyboard is a napping pad now.
    +Lou: Maybe it's writing a novel in secret.
    +Pip: I'd read anything titled "Meowmoirs".
    $ git reset --hard 01533f7
    HEAD is now at 01533f7 Add random chat log
    $ git fsck --full --unreachable | grep blob
    Checking object directories: 100% (256/256), done.
    $ git show-ref
    01533f718556a0e59f1467dae4fa462eed82c2a1 refs/heads/master
    $ git ls-files --stage
    100755 d7b4a371ebd23e682ffebc7ec355690fdc94fbd1 0       client
    100644 aa1cc01687b4ec94faf9916c3fc6efd83f23b816 0       logs/1.txt
    100644 f150f0b963ab3ee95ba5656212abd76d7f2fed2e 0       logs/2.txt
    100644 66273877d2ff3f51a14473b7200aae5a798ff64f 0       logs/4.txt
    100755 71fd2fafcd5ebd62fbf857769c92a91225ab3954 0       server
    $ git verify-pack -v .git/objects/pack/*.idx | grep blob
    fatal: Cannot open existing pack file '.git/objects/pack/*.idx'

Noticed there is one file 3.txt missing. Searching for that file and found the flag:

    $ git rev-list --all | xargs -I {} git ls-tree -r {} | grep "3.txt"
    100644 blob 7178644433e7cb6da3adf028f1c80d382a18e7b6    logs/3.txt
    $ git cat-file -p 7178644433e7cb6da3adf028f1c80d382a18e7b6
    Rex: Meet at the old arcade basement for the secret hideout.
    Jay: Ask Rusty at the door and use password picoCTF{g17_r35cu3_*******}.
    Rex: Bring the decoder map so we can plan the route.
