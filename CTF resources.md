# CTF resources

About CTF:  
For beginners:  
https://www.stationx.net/ctf-challenges-for-beginners/

CTF Handbook:  
https://ctf101.org/binary-exploitation/what-is-a-format-string-vulnerability/

CTF tools  
Free password hash cracker: https://crackstation.net/  
CyberChef: https://gchq.github.io/CyberChef/  
All Hash Generator: https://www.browserling.com/tools/all-hashes  
Generate hash: https://randommer.io/Hash/SHA256  
Solve simple chryptographic codes  
decode  
Steganography in pictures:  
https://aperisolve.fr/

Binary operations calculator:  
https://www.rapidtables.com/calc/math/binary-calculator.html

Windows Event Log analysis:  
https://andrejtopalov.medium.com/ctf-challenge-writeup-picoctf-event-viewing-8901d1f5667d  
Since .evtx files are not directly readable, my first step was to convert the file into a human-readable format using evtx_dump.py:  
evtx_dump.py Windows_Logs.evtx > logs.xml

Webapp Exploitation  
https://exploit-notes.hdks.org/exploit/web/framework/python/flask-jinja2-pentesting/  
https://www.onsecurity.io/blog/server-side-template-injection-with-jinja2/

Server Side Template Injection:  
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection  
SSTI with Jinja2:  
https://www.onsecurity.io/blog/server-side-template-injection-with-jinja2/  
https://book.hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/jinja2-ssti.html?highlight=ssti#jinja2-ssti  
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection

Payloads All The Things  
A list of useful payloads and bypasses for Web Application Security:  
https://github.com/swisskyrepo/PayloadsAllTheThings

HackTricks wiki:  
https://book.hacktricks.wiki/

RSA cipher:  
https://www.dcode.fr/rsa-cipher  
Historiske cipher og tools:  
https://www.cryptool.org/en/cto/

Local tools:  
OS Linux: Kali eller Ubuntu?  
Python, PowerShell, Bash, etc., and tools like Nmap, MetaSploit, Wireshark, Burp Suite, Ghidra, OlyDbg, IDA, pwntools  
Keepnote - notater med bilder  
Greenshot - Screengrabber + tools

Reverse shell payloads:  
https://github.com/d4t4s3c/OffensiveReverseShellCheatSheet

CIDR to IPv4 Conversion (Netmask og IP-adresser):  
https://www.ipaddressguide.com/cidr

hexdump -C yourfile.bin  
https://shell-storm.org/online/Online-Assembler-and-Disassembler/  
https://iamkate.com/code/binary-file-viewer/  
Laste ned Binary Ninja: https://binary.ninja/free/

bypass stack canary  
https://ctf101.org/binary-exploitation/stack-canaries/  
Bypassing PIE, NX and ASLR  
https://www.appknox.com/blog/bypassing-pie-nx-and-aslr  
CryptoCat: 8: Leak PIE (bypass) and Lib-C (ret2system) - Buffer Overflows - Intro to Binary Exploitation (Pwn)  
https://www.youtube.com/watch?v=NAUA1EB-TZg

Generate String from regex  
https://onlinestringtools.com/generate-string-from-regex  
Test regexp på en String:  
https://regexr.com/  
Regexp Generator:  
https://regex-generator.olafneumann.org/  
Regular Expressions 101:  
https://regex101.com/  

Base64-encoding:  
https://www.base64encode.org/

Forensics on image files:  
$exiftool upz.png  
$ xxd computer.jpg  
$ strings computer.jpg  
$ binwalk upz.png

CTF tips and tricks:  
https://infosecwriteups.com/beginners-ctf-guide-finding-hidden-data-in-images-e3be9e34ae0d  
https://medium.com/@Asm0d3us/part-1-php-tricks-in-web-ctf-challenges-e1981475b3e4  
https://davidhamann.de/2022/05/14/bypassing-regular-expression-checks/  
https://medium.com/@pkhuyar/the-danger-of-php-eval-a23410187ca2  
https://security.stackexchange.com/questions/275766/how-to-bypass-ascii-letters-and-run-the-code-in-eval  

Youtube:  
https://www.youtube.com/watch?v=qpyRz5lkRjE  
$ dmesg | tail -n 2 # lese ut meldinger om program som har krasjet

Kali:  
Gobuster is a tool used to brute-force: URIs (directories and files) in web sites, DNS subdomains (with wildcard support), Virtual Host names on target web servers, Open Amazon S3 buckets, Open Google Cloud buckets and TFTP servers:  
https://www.kali.org/tools/gobuster/

Flask Session Cookie Decoder:  
https://www.kirsle.net/wizards/flask-session.cgi

Rust playground:
https://play.rust-lang.org/
