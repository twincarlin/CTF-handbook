# MultiCode — picoCTF 2026

Download the encoded message:

    wget https://challenge-files.picoctf.net/c_plain_mesa/4d704277dbae3e29292d21448721b73ceede014eec796d45dc59ceb945f649f6/message.txt

View it:

    cat message.txt

The content is clearly wrapped in multiple layers of encoding.  
By inspecting the structure, you can identify the sequence:

1. **Base64**
2. **Hex**
3. **URL encoding**
4. **ROT13**

Using CyberChef:  
https://cyberchef.org/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)From_Hex('None')URL_Decode(true)ROT13(true,true,false,13)&input=TmpNM05qY3dOakkxTURRM05UTXlOVE0zTkRJMk1UY3lOalkyTnpjeU56RTFaamN5TmpFM01ETXdOekUzTmpZeE56UTFaak00TXpRek9EWmxNelF6TmpNMk5tWXlOVE0zTkRRPQ

After applying the transformations in this order, CyberChef reveals the final decoded flag.
