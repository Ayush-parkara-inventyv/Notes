## Overview
Suricata is an open-source, high-performance Network IDS, IPS, and Network Security Monitoring (NSM) engine designed to detect and prevent intrusions in real-time. It is capable of performing traffic analysis and packet logging for a wide variety of protocols, including TCP, UDP, HTTP, DNS, and many others. Suricata integrates with other security tools to provide a complete solution for monitoring and defending against network-based threats.

Suricata provides advanced features such as multi-threading, a flexible rule-based detection engine, and the ability to analyze network traffic at high speeds. It can be deployed in various environments, from small networks to large-scale enterprise setups.

## Key Terminology
- **IDS (Intrusion Detection System)**: A system that monitors network traffic for suspicious activity or potential security threats and alerts administrators when a potential attack is detected.
- **IPS (Intrusion Prevention System)**: Similar to IDS but with the added ability to block malicious traffic in real-time based on predefined security rules.
- **NSM (Network Security Monitoring)**: A set of tools and processes designed to monitor network traffic for signs of compromise, anomalies, or potential threats.
- **Packet Logging**: Suricata can capture packets and log them to disk, allowing for deeper analysis of network traffic, forensics, and threat hunting.
- **Signature-Based Detection**: The use of predefined rules or signatures to detect known threats in network traffic.
- **Anomaly-Based Detection**: Detecting unknown or novel threats by analyzing deviations from established normal traffic patterns.

---

## Basic Suricata Commands and Functions
### Installing Suricata
#### On Debian-based Systems (Ubuntu, Kali Linux):
```bash
sudo apt update
sudo apt install suricata
````

#### On Red Hat-based Systems (CentOS, RHEL):

```bash
sudo yum install suricata
```

#### On macOS (Using Homebrew):

```bash
brew install suricata
```

### Starting Suricata in IDS Mode

To run Suricata in IDS mode (where it only alerts on threats without blocking traffic), use the following command:

```bash
sudo suricata -c /etc/suricata/suricata.yaml -i eth0
```

- `-c /etc/suricata/suricata.yaml`: Specifies the path to the configuration file.
- `-i eth0`: Specifies the network interface to capture traffic from (replace `eth0` with the actual interface on your system).

### Running Suricata in IPS Mode

To run Suricata in IPS mode (where it can actively block malicious traffic), you need to configure it with a firewall or network access control system (e.g., iptables):

```bash
sudo suricata -c /etc/suricata/suricata.yaml --runmode ips -i eth0
```

### Viewing Suricata Logs

Suricata logs its findings into various log files. Common log types include:

- **eve.json**: A JSON-formatted log containing detailed information about network traffic, alerts, and events.
- **stats.log**: Contains performance statistics about Suricata's operation.
- **suricata.log**: General logging information about Suricata's startup and operation.

To view the logs in real-time, use `tail`:

```bash
tail -f /var/log/suricata/eve.json
```

---

## Advanced Suricata Features

### Signature-Based Detection with Suricata Rules

Suricata uses rules to detect malicious traffic. These rules are based on known attack patterns, such as specific payloads or packet sequences. Rules are written in the Suricata rule format, which is similar to Snort rules.

To use custom rules:

1. Place custom rules in the `/etc/suricata/rules/` directory.
2. Edit the `suricata.yaml` configuration file to include your rule set.
3. Restart Suricata for the rules to take effect:

```bash
sudo systemctl restart suricata
```

### Anomaly-Based Detection in Suricata

Suricata can be configured to use anomaly detection techniques by enabling `af_packet` and `flow` modules in the configuration file. These modules analyze traffic flows and detect patterns that differ from normal behavior.

### Suricata with Zeek (formerly Bro)

Suricata can be used alongside Zeek to provide comprehensive network security monitoring. While Suricata focuses on traffic analysis and intrusion detection, Zeek provides more granular analysis of network protocols. Together, they form a powerful combination for network monitoring.

### Suricata Alerts and Actions

Suricata can trigger alerts based on predefined signatures, such as when a packet matches a rule. The alerts are logged in the `eve.json` file. You can also configure Suricata to take actions, such as dropping malicious packets or blocking connections.

Example of an alert rule:

```bash
alert http any any -> any any (msg:"Possible SQL Injection"; content:"' OR 1=1"; sid:100001;)
```

This rule generates an alert when an HTTP request contains a SQL injection attempt (`' OR 1=1`).

### Suricata Performance Tuning

To optimize Suricata's performance, you can modify several configuration options:

- **Threading**: Suricata supports multi-threading to handle large volumes of traffic. You can specify the number of threads in the configuration file.
- **Buffer Size**: Increase the capture buffer size to improve packet capture performance on high-traffic networks.
- **Hardware Offloading**: Suricata can offload certain tasks to hardware, such as using specialized network cards that support high-speed packet capture.

### Suricata with Elasticsearch, Logstash, and Kibana (ELK Stack)

For advanced log analysis and visualization, Suricata can integrate with the ELK stack:

1. **Elasticsearch** stores Suricata logs in a searchable database.
2. **Logstash** parses and enriches the logs.
3. **Kibana** provides a user interface for visualizing and querying the logs.

This setup is useful for large-scale network security monitoring, where real-time analysis and historical data are required.

---

## Practical Use Cases

- **Network Intrusion Detection**: Suricata is widely used as an IDS to monitor network traffic for signs of malicious activity and generate alerts when such activity is detected.
- **Intrusion Prevention**: Suricata can function as an IPS, blocking malicious traffic in real-time based on predefined rules.
- **Network Security Monitoring (NSM)**: Suricata helps organizations maintain visibility into network traffic, providing real-time alerts and logs to support incident response and forensics.
- **Traffic Analysis**: Suricata is useful for deep packet inspection and identifying suspicious traffic patterns, such as botnets, DDoS attacks, or data exfiltration attempts.
- **Penetration Testing**: During a pentest, Suricata can be used to capture and analyze traffic, helping testers identify attack vectors, vulnerabilities, and potential exploits.

---

## Mitigation Strategies

- **Regularly Update Signatures**: Regularly update Suricata’s signature database to ensure that it can detect the latest known threats and attack patterns.
- **Limit the Scope of IPS**: In environments where performance is a concern, consider running Suricata in IDS mode initially and only enabling IPS mode for critical network segments.
- **Use Traffic Segmentation**: Segment the network to reduce the impact of attacks and to make it easier to monitor and secure individual segments.
- **Deploy Suricata with Firewalls**: Integrate Suricata with firewalls to automatically block malicious traffic that matches predefined signatures.

---

## References

- [Suricata Official Website](https://suricata-ids.org/)
- [Suricata Documentation](https://suricata.readthedocs.io/en/latest/)
- [Suricata GitHub Repository](https://github.com/OISF/suricata)

