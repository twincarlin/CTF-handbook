# Ph4nt0m 1ntrud3r — picoCTF 2025

Hints:
1. Filter your packets to narrow down your search.
2. Attacks were done in timely manner.
3. Time is essential.

I load the pcap file in Wireshark.

I observe many single TCP packets with no session tying them together.

It helps to remember the OSI model.

I sort by time (based on the hints).

I notice the payload size, and that packets with length 12 look like Base64.

Right‑click on the TCP payload → Show Packet Bytes → Decode as Base64.  
This shows the beginning of the flag.

I paste the pieces together to reconstruct the full flag:

    picoCTF + {1t_w4s + nt_th4t + _34sy_t + bh_4r_d + 4b57909 + }
