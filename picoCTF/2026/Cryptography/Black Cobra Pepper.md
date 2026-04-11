# Black Cobra Pepper — picoCTF 2026

**Unsolved**

Hints given:
1. Does this remind you of any other popular encryption?
2. What purpose does the s-box serve?

Possible hint in the code given:

    $ cat chall.py | grep "^#"
    #adopted and insipred by the code from the wikipedia article Rijndael MixColumns.

I notice that the code does not implement sbox, since the functions `sub_word`, `rcon` and `sub_bytes` returns the input without change. Therefore the AES is not correct and becomes linear.

The given values are:

    pt1 = "72616e646f6d64617461313131313131"
    ct1 = "d7481d89f1aaf5a857f56edd2ae8994c"
    ct2 = "8c7d66558130eb5796d131beb43c9934"

`pt1` is `randomdata111111` when converted **From Hex** in Cyberchef. Since all are the same length I guess the flag is also 16 characters, `picoCTF{???????}`. The padding with 1's in `pt1` might hint to that something else is padded, maybe the key.
I tried brute force the key with peppers padded with 1's, but did not find it.

Probably I can use the fact that the encoding is a linear and the known pt/ct pair to find pt2 somehow.
