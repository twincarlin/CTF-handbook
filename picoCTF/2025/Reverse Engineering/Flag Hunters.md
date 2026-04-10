# Flag Hunters — picoCTF 2025

Hints:
1. This program can easily get into undefined states. Don't be shy about Ctrl‑C.
2. Unsanitized user input is always good, right?
3. Is there any syntax that is ripe for subversion?

I test the program and see that the input is used to print output.

I use Ctrl‑C.

I download and read through the source code.

From the code I understand that it must have mechanisms for:
1. Printing line by line.
2. Jumping to the chorus between verses (and returning to where we were in the song).
3. Taking input → Crowd and using it.
4. Exiting.
5. Jumping to wherever we want (the secret intro).
