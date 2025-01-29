## Overview
Aircrack-ng is a suite of open-source tools designed for wireless network security auditing. It is primarily used for the testing of Wi-Fi networks by performing tasks such as packet sniffing, WEP/WPA key cracking, and packet injection. Aircrack-ng supports a variety of wireless network cards and operates on both Linux and macOS platforms, with limited support for Windows.

The main goal of Aircrack-ng is to test the security of wireless networks, ensuring they are properly secured and protected from common attacks, such as cracking WEP/WPA keys and sniffing sensitive data over the air.

## Key Terminology
- **WEP (Wired Equivalent Privacy)**: An outdated encryption protocol for Wi-Fi that can be cracked easily with modern tools like Aircrack-ng.
- **WPA (Wi-Fi Protected Access)**: A more secure encryption protocol than WEP, used to secure Wi-Fi networks. WPA2 is the most widely used version.
- **Packet Sniffing**: The process of capturing wireless traffic to analyze network packets for vulnerabilities, credentials, or other sensitive information.
- **Packet Injection**: The process of sending custom packets to a network in order to cause a reaction, such as re-authentication or triggering specific vulnerabilities.
- **Cracking**: The process of trying to recover the encryption keys (WEP/WPA keys) by using algorithms that attempt different combinations based on captured network traffic.

---

## Basic Aircrack-ng Commands and Functions
### Installing Aircrack-ng
#### On Debian-based Systems (Ubuntu, Kali Linux):
```bash
sudo apt update
sudo apt install aircrack-ng
````

#### On Red Hat-based Systems (CentOS, RHEL):

```bash
sudo yum install aircrack-ng
```

#### On macOS (Using Homebrew):

```bash
brew install aircrack-ng
```

### Listing Available Wireless Interfaces

To list the available wireless network interfaces on your system:

```bash
iwconfig
```

### Enabling Monitor Mode

Before capturing packets, the wireless network interface must be switched into monitor mode. This allows the interface to capture all packets in the air, even those not destined for your device.

```bash
sudo airmon-ng start wlan0
```

This command starts monitor mode on the `wlan0` interface (replace `wlan0` with the actual interface name).

### Scanning for Wireless Networks

To scan for available wireless networks (SSID, channel, encryption type, etc.), use the following command:

```bash
sudo airodump-ng wlan0mon
```

- `wlan0mon`: The wireless interface in monitor mode.

This will display a list of nearby networks, their security protocols (WEP, WPA, WPA2), and other relevant information.

### Capturing Packets

To capture packets from a specific network, specify the channel and BSSID (MAC address of the AP):

```bash
sudo airodump-ng --bssid <BSSID> -c <channel> -w capture wlan0mon
```

- `<BSSID>`: The MAC address of the target access point.
- `<channel>`: The wireless channel used by the target AP.
- `-w capture`: Specifies the filename for saving the captured packets.

### Cracking WEP/WPA Keys

#### Cracking WEP Keys

To crack a WEP key, you first need to capture enough IVs (Initialization Vectors). Once you have sufficient data, use the following command:

```bash
sudo aircrack-ng capture-01.cap
```

- `capture-01.cap`: The captured packet file.

Aircrack-ng will attempt to recover the WEP key based on the captured packets.

#### Cracking WPA/WPA2 Keys

To crack WPA/WPA2 keys, you'll need to capture a WPA handshake, which occurs when a device connects to the network. Once the handshake is captured, use the following command:

```bash
sudo aircrack-ng -w /path/to/wordlist.txt capture-01.cap
```

- `-w /path/to/wordlist.txt`: Specifies the path to the wordlist used for brute-forcing the WPA/WPA2 key.
- `capture-01.cap`: The captured handshake file.

Aircrack-ng will attempt each password from the wordlist to find the correct WPA key.

---

## Advanced Aircrack-ng Features

### Performing a Deauthentication Attack

A deauthentication attack can be used to force a client to disconnect from an access point, enabling you to capture a WPA handshake during the reauthentication process.

Use the following command:

```bash
sudo aireplay-ng --deauth 10 -a <BSSID> wlan0mon
```

- `--deauth 10`: Sends 10 deauthentication packets to the target.
- `<BSSID>`: The MAC address of the target access point.
- `wlan0mon`: The wireless interface in monitor mode.

### Packet Injection for WEP Cracking

To accelerate the process of cracking WEP, you can inject fake authentication packets to generate more IVs. Use the following command:

```bash
sudo aireplay-ng --fakeauth 0 -a <BSSID> -h <your_mac> wlan0mon
```

- `--fakeauth 0`: Sends fake authentication packets.
- `-a <BSSID>`: The MAC address of the target access point.
- `-h <your_mac>`: Your own MAC address.

This will help collect more IVs to crack the WEP key more quickly.

### Capturing Handshakes for WPA Cracking

To capture WPA handshakes, monitor network traffic to identify when a client connects to the target access point. When a client connects, a handshake is captured, which is necessary for WPA key cracking.

Use `airodump-ng` to capture handshakes:

```bash
sudo airodump-ng --bssid <BSSID> -c <channel> --write capture wlan0mon
```

- After capturing a handshake, use `aircrack-ng` with a wordlist to attempt cracking the WPA/WPA2 key.

---

## Practical Use Cases

- **Wi-Fi Penetration Testing**: Aircrack-ng is commonly used by penetration testers to assess the security of wireless networks. By testing the effectiveness of encryption protocols (e.g., WEP, WPA2), they can identify vulnerabilities that may need to be addressed.
- **WEP Cracking**: WEP is an outdated and insecure encryption protocol. Aircrack-ng is highly effective at cracking WEP keys due to weaknesses in the WEP encryption algorithm.
- **WPA/WPA2 Cracking**: Aircrack-ng can be used to brute-force WPA/WPA2 passwords by capturing WPA handshakes and using wordlists to guess the password.
- **Network Auditing**: Network administrators can use Aircrack-ng to assess the strength of their wireless security, check for weak or easily crackable passwords, and ensure proper encryption methods are being used.

---

## Mitigation Strategies

- **Use WPA2 or WPA3 Encryption**: Always use WPA2 (or WPA3 if available) encryption for Wi-Fi networks, as these protocols are much stronger than WEP and resistant to cracking by tools like Aircrack-ng.
- **Use Strong Passwords**: Ensure that Wi-Fi passwords are long, complex, and unique. The stronger the password, the harder it is to crack, even with large wordlists.
- **Disable WEP Encryption**: WEP is no longer secure and should be disabled on all modern wireless networks. Replace it with WPA2 or WPA3.
- **Disable WPS**: Wi-Fi Protected Setup (WPS) is vulnerable to brute-force attacks and should be disabled to avoid easy exploits.
- **Monitor Wireless Traffic**: Regularly monitor your network for suspicious activity, such as unauthorized devices attempting to connect or abnormal traffic patterns.

---

## References

- [Aircrack-ng Official Website](https://www.aircrack-ng.org/)
- [Aircrack-ng Documentation](https://github.com/aircrack-ng/aircrack-ng/wiki)
- [Aircrack-ng GitHub Repository](https://github.com/aircrack-ng/aircrack-ng)
