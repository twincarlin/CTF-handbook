# Rogue Tower — picoCTF 2026

Hints given:
1. Look for unauthorized test network broadcasts on UDP port 55000
2. Find the device that connected to the rogue tower by checking HTTP User-Agent headers
3. The encryption key is derived from the victim device's IMSI
4. The exfiltrated data is split across multiple HTTP POST requests

Identify the interesting traffic:

    $ tshark -r rogue_tower.pcap | grep 10.100.97.10
    15  16.974445 10.100.97.10 → 8.8.8.8      DNS 80 Standard query 0x0000 A device-310410073710709.network.com
    16  17.274445 10.100.97.10 → 198.51.100.8 HTTP 194 GET /api/register HTTP/1.1
    17  17.974445 10.100.97.10 → 198.51.100.8 HTTP 153 POST /upload HTTP/1.1
    18  18.360645 10.100.97.10 → 198.51.100.8 HTTP 153 POST /upload HTTP/1.1
    19  18.839039 10.100.97.10 → 198.51.100.8 HTTP 153 POST /upload HTTP/1.1
    20  19.312300 10.100.97.10 → 198.51.100.8 HTTP 153 POST /upload HTTP/1.1
    21  20.011472 10.100.97.10 → 198.51.100.8 HTTP 153 POST /upload HTTP/1.1
    22  20.459956 10.100.97.10 → 198.51.100.8 HTTP 147 POST /upload HTTP/1.1

IMSI is probably the 15-digit number here: device-310410073710709. I see the other devices have similar starting with the same 6-7 digits.

Compile the exfiltrated data in the HTTP POST requests:

    $ tshark -r rogue_tower.pcap -Y "http.request.method == POST" -T fields -e http.file_data | tr -d '\n'
    52317055586e4e6a646b4a464131424541326854436c74666145554151414e4c61464a57557764535667465454673d3d

Test this in Cyberchef with From Hex and From Base64. Guess is that the encrytion is XOR, but it does not work with the IMSI. 
Hint 3 says the encryption key is *derived from*. 
Testing XOR with `picoCTF{` I see that this corresponds to the last 8 digits in the IMSI. 
When I use these 8 digits as XOR key we get the flag, `picoCTF{r0gu3_c3ll_t0w3r_********}`, which seems to match the title and description.
