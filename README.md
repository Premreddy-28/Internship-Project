## Network Scanning & Packet Analysis with Nmap and Wireshark

This project demonstrates how I performed a **TCP SYN scan** on a local network using **Nmap** and analyzed the captured packets using **Wireshark** to understand how scanning works at the packet level.The scan targeted a vulnerable machine (Metasploitable 2) in my virtual lab.

## Objective

To simulate a basic reconnaissance activity in a penetration testing workflow:
- Identify active devices on the network
- Discover open ports and services
- Understand TCP SYN scanning at packet level

## Lab Setup

| Component        | Details                     |
|------------------|-----------------------------|
| Attacker Machine | Kali Linux                  |
| Target Machine   | Metasploitable 2            |
| Target IP        | 192.168.29.188              |
| Network Range    | 192.168.29.0/24             |
| Tools Used       | Nmap, Wireshark             |

## Step-by-Step Execution

 1. Identify Local Network Range
     - Used `ifconfig` to identify my local IP and determine the subnet: 192.168.29.0/24

 3. Run Nmap TCP SYN Scan
     - `nmap -sS 192.168.29.0/24`
     - -sS = TCP SYN scan (also known as a stealth scan)
     - This scan attempts to detect open TCP ports without completing the handshake.

 4. Analyze Packets with Wireshark
     - Opened Wireshark while running the Nmap scan
     - Applied filter: `tcp.flags.syn == 1 && tcp.flags.ack == 0`
     - This filter shows only SYN packets initiated by the scanner
 
 5. SYN Packet List in Wireshark
    - Shows multiple SYN packets sent by Nmap to scan TCP ports

##  Key Findings

The Nmap SYN scan on 192.168.29.188 (Metasploitable VM) revealed multiple open TCP ports. These shows the services that could be vulnerable if misconfigured or outdated.

### Discovered Open Ports:

| Port  | Service         | Description                                       |
|-------|------------------|---------------------------------------------------|
| 21    | FTP              | May allow anonymous login                        |
| 22    | SSH              | Secure shell; commonly targeted for brute force  |
| 23    | Telnet           | Insecure remote access protocol                  |
| 25    | SMTP             | Mail server, potentially open relay              |
| 53    | DNS              | DNS resolver; could be vulnerable to poisoning   |
| 80    | HTTP             | Web service; often vulnerable in test VMs        |
| 111   | RPCBind          | Remote Procedure Call services                   |
| 139   | NetBIOS-SSN      | File sharing (Windows systems)                   |
| 445   | Microsoft-DS     | SMB over TCP; used for file sharing              |
| 512   | exec             | Remote command execution                         |
| 513   | login            | Remote login service                             |
| 514   | shell            | Insecure remote shell                            |
| 1099  | RMI Registry     | Java RMI service                                 |
| 1524  | ingreslock       | Often backdoor port in Metasploitable            |
| 2049  | NFS              | Network File System                              |
| 2121  | ccproxy-ftp      | FTP proxy port                                   |
| 3306  | MySQL            | Database service                                 |
| 5432  | PostgreSQL       | PostgreSQL database                              |
| 5900  | VNC              | Remote desktop service                           |
| 6000  | X11              | GUI display protocol                             |
| 6667  | IRC              | Internet Relay Chat                              |
| 8009  | AJP13            | Apache JServ Protocol (Tomcat)                   |
| 8180  | Unknown          | Possibly another HTTP/Tomcat port                |

## Security Note:  
Most of these ports are intentionally left open in Metasploitable to simulate vulnerable services. In real-world environments services like Telnet, FTP, and open databases may occur serious risks if exposed to the internet.

## Conclusion
This project helped me understand how TCP SYN scanning works and how to analyze network traffic using Wireshark. I identified open ports on a vulnerable machine interpreted the results and connected them to real-world security risks. It gave me hands-on experience with essential tools and strengthened my foundation in ethical hacking and network security.
