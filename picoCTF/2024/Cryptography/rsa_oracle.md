# rsa_oracle — picoCTF 2024

We cannot ask the oracle to decrypt the password (call it m), but we *can* ask it to decrypt anything else.

Idea:  
We can ask the oracle to decrypt **2m**, because we know the ciphertext of m.

We use the RSA property:

    (2^e mod n) * (m^e mod n) = (2m)^e mod n

So we can let the oracle decrypt this product and then divide by 2 to recover m.

Find (2^e mod n):

Use CyberChef to find the hex string that represents the number 2:  
https://gchq.github.io/CyberChef/#recipe=From_Hex('Auto')&input=Mg

Decrypt this hex using the oracle and get:

    5067313465613043651275429665315895824157755779222372979446076012356324498190828210335763979330272318657269048435311897896433721115606764442199497891219230

Multiply this with the encoded password (from the provided file) using an online big number calculator:  
https://www.dcode.fr/big-numbers-multiplication

Result:

    8283377309369758183523145573200750782484310290735315558059488823972896636253855296781020106079552858693546617489082932997730796347166223270632814660216969565981265960585467439881131219657789014058412904318589178439314193411103546217783791481965173915838239819656233269969451945419284153208337289812023220310

Decrypt this (which is 2m) using the oracle:

    Hex: 68726a6aca
    String: hrjjÊ

Divide the hex result by 2:  
https://www.calculator.net/hex-calculator.html?number1=68726a6aca&c2op=%2F&number2=2&calctype=op&x=Calculate

Result:

    68726a6aca ÷ 2 = 3439353565

Decode this hex to string in CyberChef:  
https://gchq.github.io/CyberChef/#recipe=From_Hex('Auto')&input=MzQzOTM1MzU2NQ&oeol=FF

Result:

    4955e

Use this as the key to decrypt the secret:

    openssl enc -aes-256-cbc -d -in secret.enc -k 4955e

Output:

    picoCTF{su((3ss_(r@ck1ng_r3@_********}
