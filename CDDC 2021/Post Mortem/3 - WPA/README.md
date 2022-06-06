# WPA

## Challenge Description

The resistance has successfully intercepted encrypted WiFi traffic from a Cyber-BOT believed to communicate with the HQ. It is your task as a security analyst and cyber expert to decrypt the traffic and find out if there is any valuable information about the communication.

## Attached files

* pcap.cap

---

## Solution

First, we can open the file in Wireguard. As expected, since it is encrypted, we cannot make sense of the data:

<img width="1332" alt="Screenshot 2021-06-24 at 15 12 01" src="https://user-images.githubusercontent.com/40383042/126428896-5f009ab7-a503-46f2-901d-08d66da708c8.png">

To decrypt the traffic, we must first make sure that the data contains the 4-way handshake, which is needed for us to crack the key:

<img width="1332" alt="Screenshot 2021-06-24 at 11 10 5111117" src="https://user-images.githubusercontent.com/40383042/126428922-dbd105d8-cb8b-4579-bbed-cb0c3d742485.png">

We can then crack the key using `aircrack-ng`:

```text
$ aircrack-ng -c pcap.cap -w /usr/share/wordlists/rockyou.txt
Reading packets, please wait...
Opening pcap.cap
Read 892 packets.

   #  BSSID              ESSID                     Encryption

   1  18:A6:F7:2C:AC:DA  CyBots                    WPA (1 handshake)

Choosing first network as target.

Reading packets, please wait...
Opening pcap.cap
Read 892 packets.

1 potential targets



                               Aircrack-ng 1.6 

      [00:00:00] 294/10303727 keys tested (1473.24 k/s) 

      Time left: 1 hour, 56 minutes, 33 seconds                  0.00%

                          KEY FOUND! [ 0123456789 ]


      Master Key     : 48 CE BF FD 15 0D 75 B7 AB F3 2B 93 F4 25 34 5C 
                       93 74 F0 F3 2C 60 DB 9F 30 AB D5 C2 9E 5C FF FD 

      Transient Key  : EF 4F C4 2E 0D 12 0C 4C C4 D4 2E 93 E7 56 53 01 
                       D5 B1 22 B4 4A 32 13 00 00 00 00 00 00 00 00 00 
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 

      EAPOL HMAC     : B7 DF 89 96 BD 55 04 7A 41 32 7D 8D 93 84 A4 E1 

```

In this case, I used the `rockyou` wordlist, provided with Kali.

Aircrack managed to get the key fairly easily &ndash; let's go back to Wireshark to decrypt the capture. First, let us enter the key into Wireshark, by going to `Settings > Protocols > IEEE 802.11`:

<img width="858" alt="Screenshot 2021-07-21 at 10 15 08" src="https://user-images.githubusercontent.com/40383042/126428957-5d23b966-c817-474e-914e-d646f3fe8a7b.png">

For WPA, we enter `key:SSID` &ndash; the key was cracked earlier, and the SSID can be found earlier:

<img width="688" alt="Screenshot 2021-06-24 at 11 10 5sdfdf111117" src="https://user-images.githubusercontent.com/40383042/126428968-30517ed1-a647-423b-95b5-38b8fe6669e9.png">

> **Note:** IP addresses redacted.

Then, we can look at the decrypted traffic. Upon inspecting it, we can find some HTTP traffic.

<img width="1332" alt="Screenshot 2021-06-24 at 15 16 40" src="https://user-images.githubusercontent.com/40383042/126428980-7e499f34-e985-482c-96a1-b85e12d1b74e.png">

<img width="1088" alt="Screenshot 2021-06-24 at 15 17 32" src="https://user-images.githubusercontent.com/40383042/126429073-82d96a84-5f56-48fe-ac4c-cf728a51047c.png">

This looks like where we should be looking, but the flag itself isn't here. We can try visiting the source of the traffic ourselves:

<img width="534" alt="Screenshot 2021-06-24 at 15 18 35" src="https://user-images.githubusercontent.com/40383042/126429049-8fca8ea8-2053-4da2-ba74-630f416cb82d.png">

### Flag

```text
CDDC21{-DecRypted_WPA-}
```
