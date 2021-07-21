# WPA

## Challenge Description
The resistance has successfully intercepted encrypted WiFi traffic from a Cyber-BOT believed to communicate with the HQ. It is your task as a security analyst and cyber expert to decrypt the traffic and find out if there is any valuable information about the communication.

## Attached files
* pcap.cap

---

## Solution
First, we can open the file in Wireguard. As expected, since it is encrypted, we cannot make sense of the data:
<!--screenshot-->

To decrypt the trafic, we must first make sure that the data contains the 4-way handshake, which is needed for us to crack the key:
<!--screenshot-->

We can then crack the key using `aircrack-ng`:
```
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

Aircrack managed to get the key fairly easily – let's go back to Wireshark to decrypt the capture. First, let us enter the key into Wireshark, by going to `Settings > Protocols > IEEE 802.11`:
<!--screenshot-->

For WPA, we enter `key:SSID` – the key was cracked earlier, and the SSID can be found earlier:
<!--screenshot-->

Then, we can look at the decrypted traffic. Upon inspecting it, we can find some HTTP traffic.
<!--screenshot-->

We can then follow the HTTP stream.
<!--screenshot-->

This looks like where we should be looking, but the flag itself isn't here. We can try visiting the source of the traffic ourselves (IP redacted):
<!--screenshot-->

### Flag:
```
CDDC21{-DecRypted_WPA-}
```
