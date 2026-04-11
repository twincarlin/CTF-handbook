# Blast from the past — picoCTF 2024

Inspect the file to understand its metadata layout:

    exiftool -v3 original.jpg

Look at the raw bytes to locate timestamp fields:

    bvi original.jpg

Convert the file to a plain hex stream for easier analysis:

    xxd -p original.jpg

After identifying the correct timestamp field, modify it:

    exiftool -AllDates='1970:01:01 00:00:00.001' original.jpg

(I also tried SubSecCreateDate, SubSecDateTimeOriginal, SubSecModifyDate.)

Send the modified file to the challenge server:

    nc -w 2 mimas.picoctf.net 65080 < original.jpg

Retrieve the flag from the second port:

    nc mimas.picoctf.net 57713
