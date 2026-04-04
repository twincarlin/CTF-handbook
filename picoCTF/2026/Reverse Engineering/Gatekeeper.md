# Gatekeeper — picoCTF 2026

Inspect the binary:

    file gatekeeper
    gatekeeper: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=f8be7d9d53fcf8763ed4b76fe8c98b23609085db, for GNU/Linux 3.2.0, not stripped

Make it executable:

    chmod +x gatekeeper

A brute-force shell script was created to test all numbers starting from 1000.  
This script reached 10000 without finding anything.

Trying other inputs gives different error messages, but this one looks promising:

    ./gatekeeper
    Enter a numeric code (must be > 999 ): aaa
    Flag file not found.

Test the same input against the remote server:

    nc green-hill.picoctf.net 58760
    Enter a numeric code (must be > 999 ): aaa
    Access granted: }***ftc_oc_ip******ftc_oc_ipa_99ftc_oc_ip9_TGftc_oc_ip_xehftc_oc_ip_tigftc_oc_ipid_3ftc_oc_ip{FTCftc_oc_ipocipftc_oc_ip

Copy the output into a text editor and remove the repeating pattern `ftc_oc_ip`, resulting in:

    }*******_999_TG_xeh_tigid_3{FTCocip

Reverse the string using CyberChef:  
https://cyberchef.org/#recipe=Reverse('Character')&input=fTRjZDZmNDdhXzk5OV9UR194ZWhfdGlnaWRfM3tGVENvY2lw

The reversed and cleaned result reveals the flag
