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

Second attempt at brute force combined with the fact that encoding is linear given that s-box does not work properly.  
This time I brute force possible flag :
* the flag is 16 characters, `picoCTF{???????}`
* the last character is `!` based on the wording in the challenge description: `(change!)`
* padding with `1` (not 1337 speak) somewhere inside the word, based on padding in `pt1`
* this leaves 5 characters for a word, where I assume it is related to "Black Cobra Peppers" and "i like peppers" in the challenge description

I asked AI to create a word list based on this for me and got the following python script:
```python
# Candidates based on the "Black Cobra Pepper" (Capsicum annuum)
# Known for being 'spicy', 'fuzzy' (hairy leaves), and also called 'goats' weed.
base_words = ["cobra", "chili", "chile", "spicy", "fuzzy", "annum", "goats"]

wordlist = []

for word in base_words:
    # Insert '1' at every possible index from start to the end of the word
    # Range is len(word) + 1 to allow '1' to appear after the last letter 
    # but before the '!' suffix.
    for i in range(len(word) + 1):
        candidate = f"{word[:i]}1{word[i:]}!"
        wordlist.append(candidate)

# Print results for the wordlist file
for entry in wordlist:
    print(entry)
```
I asked for some more 5 letter words and incorporated this into a solution script:
```python
# Challenge Data
pt1_hex = "72616e646f6d64617461313131313131"
ct1_hex = "d7481d89f1aaf5a857f56edd2ae8994c"
ct2_hex = "8c7d66558130eb5796d131beb43c9934"

# Convert to integers/bytes for calculation
pt1_bytes = binascii.unhexlify(pt1_hex)
ct1_bytes = binascii.unhexlify(ct1_hex)
ct2_bytes = binascii.unhexlify(ct2_hex)

# The XOR difference we are looking for (in bytes)
target_ct_diff = bytes(a ^ b for a, b in zip(ct1_bytes, ct2_bytes))

def generate_picoctf_candidates():
    base_words = ["cobra", "chili", "chile", "spicy", "fuzzy", "annum", "goats", "black", "green", "white", "berry", "seeds", "fruit"]
    for word in base_words:
        for i in range(len(word) + 1):
            inner = f"{word[:i]}1{word[i:]}!"
            yield f"picoCTF{{{inner}}}"

print("Starting linear attack...")

dummy_key = "00" * 16 # 32-char hex string

for candidate in generate_picoctf_candidates():
    candidate_bytes = candidate.encode()
    
    # Calculate difference as bytes
    pt_diff_bytes = bytes(a ^ b for a, b in zip(pt1_bytes, candidate_bytes))
    
    # Convert difference to HEX STRING to satisfy the AES function's input
    pt_diff_hex = pt_diff_bytes.hex()
    
    # The AES function returns a hex string; convert it to bytes for comparison
    result_hex = AES(pt_diff_hex, dummy_key)
    result_bytes = binascii.unhexlify(result_hex)
    
    if result_bytes == target_ct_diff:
        print(f"\n[+] Flag Found: {candidate}")
        break
```
This gives the flag.
