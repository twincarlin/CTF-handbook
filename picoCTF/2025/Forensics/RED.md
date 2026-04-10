# RED — picoCTF 2025

I download the file with wget.

    file red.png

    strings red.png

This gives the hint: CHECKLSB.

This refers to Least Significant Bit.

I check it in CyberChef.

Using RGBA view, I see a repeating Base64 string:

    cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==

I decode it with “From Base64” and get the flag.
