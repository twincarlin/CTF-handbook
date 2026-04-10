# Quantum Scrambler — picoCTF 2025

Hints:
1. Run eval on the cypher to interpret it as a Python object.
2. Print the outer list one object per line.
3. Feed in a known plaintext through the scrambler.

I test with the known plaintext:

    123456789

I see that the first and last element of each list are what we need.

I write a Python script that reads cypher.txt and prints the first and last element of each list:

```python
    import sys
    file = open('cypher.txt', 'r')
    ll = eval(file.read())
    print(len(ll))
    i = 0
    for i in range(len(ll)):
      print(ll[i][0])
      print(ll[i][-1])
```
I paste the output into CyberChef and convert from Hex.
