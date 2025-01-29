## Overview

`sdptool` is a command-line utility in Linux used to interact with the Service Discovery Protocol (SDP) on Bluetooth devices. It allows querying, browsing, and registering SDP records to facilitate device communication.

## Key Terminology

- **SDP (Service Discovery Protocol)**: A Bluetooth protocol used to discover services available on remote devices.
- **UUID (Universally Unique Identifier)**: A 128-bit identifier used to specify Bluetooth services.
- **RFCOMM**: A transport protocol providing serial data transfer over Bluetooth.
- **L2CAP (Logical Link Control and Adaptation Protocol)**: The foundation of Bluetooth's data transmission.

---

## Basic Commands

### Checking Available Commands

```bash
sdptool --help  # Display available commands and usage
```

### Searching for Bluetooth Devices

```bash
sdptool search <service>
```

Example:

```bash
sdptool search OPUSH  # Search for devices supporting Object Push Profile (OPP)
```

### Browsing Available Services

```bash
sdptool browse <bdaddr>
```

Example:

```bash
sdptool browse 00:1A:7D:DA:71:13  # List services on the device
```

### Retrieving Service Records

```bash
sdptool records <bdaddr>
```

Example:

```bash
sdptool records 00:1A:7D:DA:71:13  # Retrieve full service records
```

---

## Advanced Commands

### Adding a Service

```bash
sdptool add <service>
```

Example:

```bash
sdptool add SP  # Add Serial Port Profile (SPP)
```

### Removing a Service

```bash
sdptool del <service>
```

Example:

```bash
sdptool del SP  # Remove Serial Port Profile
```

### Connecting to a Remote Service

```bash
sdptool connect <bdaddr> <channel>
```

Example:

```bash
sdptool connect 00:1A:7D:DA:71:13 1  # Connect to channel 1 of the device
```

### Service Availability Testing

```bash
sdptool avinfo <bdaddr>
```

Example:

```bash
sdptool avinfo 00:1A:7D:DA:71:13  # Display available services in A/V profile
```

---

## Practical Use Cases

- **Identifying supported services**: Helps in verifying if a device supports profiles like HID, A2DP, HFP, etc.
- **Debugging Bluetooth connections**: Useful for troubleshooting issues by checking available services.
- **Registering services for communication**: Enables the addition of profiles like SPP for serial communication.

---

## Troubleshooting

### Bluetooth Service Not Running

```bash
sudo systemctl start bluetooth
sudo systemctl enable bluetooth
```

### Permission Issues

```bash
sudo usermod -aG bluetooth $(whoami)  # Add user to the Bluetooth group
```

### Device Not Discoverable

```bash
hciconfig hci0 up
hciconfig hci0 piscan  # Enable scanning mode
```

---

## References

- [Linux Man Page](https://man7.org/linux/man-pages/man1/sdptool.1.html)
- [Bluetooth SIG](https://www.bluetooth.com/)
- Useful debugging tool: `hcitool` for scanning and device management.

---

> **Pro Tip:** Use `bluetoothctl` in combination with `sdptool` for a comprehensive Bluetooth management experience!