# EVEN RSA CAN BE BROKEN??? — picoCTF 2025

Hints:
1. How much do we trust randomness?
2. Notice anything interesting about N?
3. Try comparing N across multiple requests.

I read up on RSA:  
Public key is N and e.  
Private key is p, q and d.  
Security relies on the difficulty of factoring a large number into two large primes.

I run the netcat command several times to look for patterns.

I notice that N is even, but it should be odd if it is the product of two primes.

So one of the primes must be 2.

I could compute the full private key, but this is enough. I enter what I know here:

    https://www.dcode.fr/rsa-cipher
