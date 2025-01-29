## Overview
Airgeddon is a multi-use bash script designed for wireless auditing and penetration testing of Wi-Fi networks. It is an easy-to-use tool for performing common wireless security assessments, such as cracking WEP/WPA/WPA2 passwords, conducting deauthentication attacks, and testing the robustness of Wi-Fi encryption protocols. Airgeddon works with tools like Aircrack-ng, Reaver, and PixieWPS to provide a comprehensive suite for testing the security of wireless networks.

Airgeddon provides an interactive interface that makes it easy to navigate and perform a variety of attacks on Wi-Fi networks, especially for penetration testers and security professionals who need to assess the security of their Wi-Fi infrastructure.

## Key Terminology
- **WEP (Wired Equivalent Privacy)**: A legacy encryption protocol for Wi-Fi networks, which is known to be weak and easily cracked by attackers.
- **WPA/WPA2 (Wi-Fi Protected Access)**: Stronger encryption protocols used to secure Wi-Fi networks. WPA2 is the more commonly used standard today.
- **Deauthentication Attack**: A type of denial-of-service (DoS) attack where an attacker forces a device to disconnect from a Wi-Fi network, allowing for potential data interception or enabling an attack like handshake capture.
- **Handshake**: A process where a Wi-Fi client and access point exchange authentication information. Handshakes are crucial for password cracking attacks, such as those performed with Aircrack-ng or Hashcat.
- **WPS (Wi-Fi Protected Setup)**: A protocol designed to simplify the process of connecting devices to Wi-Fi networks. WPS is vulnerable to brute-force attacks, making it a target for penetration testing.

---

## Basic Airgeddon Commands and Functions
### Installing Airgeddon
To install Airgeddon, follow these steps on a Linux-based system:
1. Clone the repository from GitHub:
   ```bash
   git clone https://github.com/v1s1t0r1sh3r3/airgeddon.git
```

2. Change to the Airgeddon directory:
    
    ```bash
    cd airgeddon
    ```
    
3. Ensure all dependencies are installed:
    
    ```bash
    sudo apt update
    sudo apt install -y bc ipcalc iw net-tools sudo
    ```
    

### Running Airgeddon

To start the Airgeddon script, run:

```bash
sudo bash airgeddon.sh
```

This will launch the interactive menu, where you can select various options for performing Wi-Fi security assessments.

### Selecting the Wireless Interface

Upon starting Airgeddon, you will be prompted to select the wireless interface you wish to use. This interface must be capable of entering monitor mode. Use the following command to list available wireless interfaces:

```bash
iw dev
```

---

## Common Airgeddon Attacks

### WEP Cracking

Airgeddon can be used to crack WEP-encrypted Wi-Fi networks by capturing enough packets (usually around 10,000) and using a dictionary attack or a brute-force approach with Aircrack-ng. To initiate WEP cracking:

1. Select the target network.
2. Choose the WEP cracking option.
3. Airgeddon will begin capturing packets and attempt to crack the WEP key.

### WPA/WPA2 Cracking

For WPA or WPA2 cracking, Airgeddon will capture a handshake between the target access point and a client. After capturing the handshake, it can be used to attempt a dictionary or brute-force attack with tools like Aircrack-ng or Hashcat.

1. Select the WPA/WPA2 cracking option.
2. Once the handshake is captured, Airgeddon will prompt you to specify a wordlist for the attack.
3. Airgeddon will use the wordlist to attempt cracking the WPA password.

### WPS Brute Force Attack

Airgeddon can perform a brute-force attack on a Wi-Fi network’s WPS PIN. This is useful for networks that have WPS enabled, as WPS is known to be vulnerable to such attacks.

1. Choose the WPS attack option.
2. Airgeddon will use Reaver or Bully to attempt a brute-force attack on the WPS PIN.

### Deauthentication Attack

A deauthentication attack can be performed to disrupt communication between a Wi-Fi client and the access point. This is useful for capturing handshakes or for temporarily disabling a network to perform further attacks.

1. Select the deauthentication attack option.
2. Choose the target network and client to deauthenticate.
3. Airgeddon will broadcast deauthentication packets, causing the client to disconnect.

---

## Advanced Airgeddon Features

### WPS Pin Generator (PixieWPS)

Airgeddon integrates PixieWPS, which can be used to exploit vulnerabilities in WPS PIN generation. This tool allows for offline brute-forcing of the WPS PIN, bypassing the need to attempt an online brute-force attack.

1. After selecting the target, choose the PixieWPS option.
2. Airgeddon will attempt to generate the WPS PIN offline using the PixieWPS method.

### Wireless Network Sniffing

Airgeddon provides an option to sniff wireless networks and capture packets, including handshakes. This feature is useful for gathering data for cracking WPA keys or identifying weak networks.

1. Select the sniffing option in the main menu.
2. Airgeddon will display a list of available wireless networks.
3. Choose a target and start sniffing to capture packets and handshakes.

### Intercepting WPA Handshakes

Once a handshake is captured, it can be used to attempt a WPA key crack using wordlists. Airgeddon automatically saves the captured handshakes for later use.

---

## Practical Use Cases

- **Wireless Penetration Testing**: Airgeddon is commonly used by penetration testers to audit the security of Wi-Fi networks by attempting to crack WEP, WPA, and WPS PINs, or by performing deauthentication attacks to capture handshakes.
- **WPS Audits**: Airgeddon can be used to assess the security of Wi-Fi networks that have WPS enabled, as WPS is a vulnerable protocol that can be cracked using brute-force attacks.
- **Wi-Fi Network Analysis**: By sniffing network traffic and capturing handshakes, Airgeddon can be used to gather data for offline cracking or for assessing network vulnerabilities.
- **Network Admin Audits**: Network administrators can use Airgeddon to test their own Wi-Fi networks for vulnerabilities, ensuring that their networks are not susceptible to common attacks.

---

## Mitigation Strategies

- **Disable WEP**: WEP is highly insecure and should not be used in modern networks. Always use WPA2 or WPA3 encryption.
- **Disable WPS**: If WPS is not absolutely necessary, it should be disabled to prevent attackers from exploiting its weak PIN generation.
- **Use Strong WPA2 Passwords**: Ensure that WPA2 passwords are long and complex to prevent dictionary attacks from succeeding. A password manager can help in generating and storing secure passwords.
- **Monitor Wireless Networks**: Regularly monitor your wireless networks for unusual activity or unauthorized attempts to connect. Tools like Airgeddon can be used to conduct penetration testing.
- **Enable WPA3**: WPA3 is the latest Wi-Fi security protocol and offers enhanced protection over WPA2. Whenever possible, upgrade to WPA3 for stronger encryption.

---

## References

- [Airgeddon GitHub Repository](https://github.com/v1s1t0r1sh3r3/airgeddon)
- [Airgeddon Documentation](https://github.com/v1s1t0r1sh3r3/airgeddon/wiki)
