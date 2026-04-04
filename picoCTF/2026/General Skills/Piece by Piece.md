# Piece by Piece — picoCTF 2026

Read the instructions:

    cat instructions.txt
    - The flag is split into multiple parts as a zipped file.
    - Use Linux commands to combine the parts into one file.
    - The zip file is password protected. Use this "supersecret" password to extract the zip file.
    - After unzipping, check the extracted text file for the flag.

Concatenate all parts into a single ZIP file:

    cat part_a* > full.zip

Unzip the combined archive:

    unzip full.zip

When prompted for a password, enter:

    supersecret

Read the extracted flag:

    cat flag.txt
