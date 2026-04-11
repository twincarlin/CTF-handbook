# Mob psycho — picoCTF 2024

Unzip the APK:

    $ unzip ./mobpsycho.apk

Search for the flag:

    $ find | grep flag
    ./res/color/flag.txt

Read the file:

    $ cat ./res/color/flag.txt
    7069636f4354467b6178386d433052553676655f4e5838356c346178386d436c5f35653637656135657d

Paste the hex string into CyberChef and apply **From Hex** to get the flag.
