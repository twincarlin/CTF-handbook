# packer — picoCTF 2024

Unpack the binary:

    $ upx -d out

Search for the password message:

    $ strings out | grep "Password"
    Password correct, please see flag: 7069636f4354467b5539585f556e5034636b314e365f42316e34526933535f65313930633366337d

Paste the hex string into CyberChef and apply **From Hex** to get the answer.
