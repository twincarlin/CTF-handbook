# Password Profiler — picoCTF 2026

Download all challenge files:

    wget https://challenge-files.picoctf.net/c_plain_mesa/3c16fb8e48ddae444f6840e81660a5a8cb2d30b69092b04f619d6ac34676a919/userinfo.txt
    wget https://challenge-files.picoctf.net/c_plain_mesa/3c16fb8e48ddae444f6840e81660a5a8cb2d30b69092b04f619d6ac34676a919/hash.txt
    wget https://challenge-files.picoctf.net/c_plain_mesa/3c16fb8e48ddae444f6840e81660a5a8cb2d30b69092b04f619d6ac34676a919/check_password.py

Clone CUPP (Common User Passwords Profiler):

    git clone https://github.com/Mebus/cupp.git

Generate a password list based on the user info in `userinfo.txt`:

    cat userinfo.txt
    python3 ./cupp/cupp.py -i

Check how many passwords were generated:

    wc -l alice.txt
    5179 alice.txt

Copy the generated list to the filename expected by the checker:

    cp alice.txt passwords.txt

Run the password checker:

    python3 check_password.py
    Password found: picoCTF{Aj_15901990}
