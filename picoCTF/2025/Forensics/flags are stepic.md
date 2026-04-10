# flags are stepic — picoCTF 2025

I view the page source. I see that one flag image is scaled down:

        { name: "Upanzi, Republic The", img: "flags/upz.png",
          style:"width: 120px!important; height: 90px!important;" },

Download the images:

    wget http://standard-pizzas.picoctf.net:62103/flags/upz.png
    wget http://standard-pizzas.picoctf.net:62103/flags/no.png

Check them:

    ls -ltr
    file no.png
    file upz.png
    strings upz.png

The hint “CHECKLSB” suggests LSB steganography.

Check `stepic`:

    man stepic
    stepic -h

It runs despite warnings, but gets killed in the Webshell, so I install it locally.

Try:

    stepic -d -i upz.png -o flag.txt

This does not work.

I find this write‑up (not helpful for my case):  
https://medium.com/@erichdryn/flags-are-septic-picoctf-writeup-de2502c5a21c

Then I try Python:

    python3 -c "import warnings; from PIL import Image; import stepic; \
    warnings.filterwarnings('ignore'); Image.MAX_IMAGE_PIXELS = None; \
    print(stepic.decode(Image.open('upz.png')))"

Still no result.

Finally, I got it using a Python script as described here:  
https://medium.com/@ammarofc2005/picoctf-2025-flags-are-stepic-writeup-1d9af8c0daba
