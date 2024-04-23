# Nmap Scanning Guide

This document serves as a comprehensive guide for using Nmap, providing various scanning techniques, output options, and performance settings. This cheat sheet is helpful for both beginners and advanced users, featuring basic and advanced commands for effective network exploration and security auditing.

## Scanning Options

| **Nmap Option**          | **Description**                                             |
|--------------------------|-------------------------------------------------------------|
| `10.10.10.0/24`          | Target network range.                                       |
| `-sn`                    | Disables port scanning.                                     |
| `-Pn`                    | Disables ICMP Echo Requests.                                |
| `-n`                     | Disables DNS Resolution.                                    |
| `-PE`                    | Performs the ping scan using ICMP Echo Requests.            |
| `--packet-trace`         | Shows all packets sent and received.                        |
| `--reason`               | Displays the reason for a specific result.                  |
| `--disable-arp-ping`     | Disables ARP Ping Requests.                                 |
| `--top-ports=<num>`      | Scans the specified top ports.                              |
| `-p-`                    | Scan all ports.                                             |
| `-p22-110`               | Scan all ports between 22 and 110.                          |
| `-p22,25`                | Scans only the specified ports 22 and 25.                   |
| `-F`                     | Scans top 100 ports.                                        |
| `-sS`                    | Performs a TCP SYN-Scan.                                    |
| `-sA`                    | Performs a TCP ACK-Scan.                                    |
| `-sU`                    | Performs a UDP Scan.                                        |
| `-sV`                    | Scans the discovered services for their versions.           |
| `-sC`                    | Perform a Script Scan with scripts categorized as "default" |
| `--script <script>`      | Performs a Script Scan using the specified scripts.         |
| `-O`                     | Performs an OS Detection Scan to determine the OS.          |
| `-A`                     | Comprehensive scan (OS Detection, Services, Traceroute).    |
| `-D RND:5`               | Uses 5 random Decoys in the scan to obscure the source.     |
| `-e`                     | Specifies the network interface used for the scan.          |
| `-S 10.10.10.200`        | Specifies the source IP address for the scan.               |
| `-g`                     | Specifies the source port for the scan.                     |
| `--dns-server <ns>`      | Uses a specified name server for DNS resolution.            |

## Output Options

| **Nmap Option**  | **Description**                                        |
|------------------|--------------------------------------------------------|
| `-oA filename`   | Stores the results in all formats with prefix "filename". |
| `-oN filename`   | Stores the results in normal format with "filename".   |
| `-oG filename`   | Stores the results in "grepable" format with "filename". |
| `-oX filename`   | Stores the results in XML format with "filename".       |

## Performance Options

| **Nmap Option**               | **Description**                                      |
|-------------------------------|------------------------------------------------------|
| `--max-retries <num>`         | Sets the number of retries for scans of specific ports. |
| `--stats-every=5s`            | Displays scan's status every 5 seconds.              |
| `-v/-vv`                      | Displays verbose output during the scan.             |
| `--initial-rtt-timeout 50ms`  | Sets the initial RTT timeout to 50ms.                |
| `--max-rtt-timeout 100ms`     | Sets the maximum RTT timeout to 100ms.               |
| `--min-rate 300`              | Sets the minimum number of packets sent per second.  |
| `-T <0-5>`                    | Specifies the specific timing template for the scan. |

### Practical Examples

#### Scan Network Range
```bash
sudo nmap 10.129.2.0/24 -sn -oA tnet
```

### Scan IP List

sudo nmap -sn -oA tnet -iL hosts.lst


# Nmap Scan Techniques

## Detailed Command Descriptions

1. **Scan Network Range**: `sudo nmap 10.129.2.0/24 -sn -oA tnet`
    - Targets a network range, disables port scanning, and stores the results in all formats starting with the name 'tnet'.

2. **Scan IP List**: `sudo nmap -sn -oA tnet -iL hosts.lst`
    - Performs defined scans against targets in the provided 'hosts.lst' list.

3. **Scan Multiple IPs**: `sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20`
    - Allows scanning multiple specified IP addresses.

4. **Scan Single IP**: `sudo nmap 10.129.2.18 -sn -oA host`
    - Scans a single IP, disables port scanning, and stores the results with the prefix 'host'.

5. **Ping Scan with ICMP Echo Requests**: `sudo nmap 10.129.2.18 -sn -oA host -PE`
    - Performs a ping scan using ICMP Echo requests.

6. **Determining Host Status with Reason**: `sudo nmap 10.129.2.18 -sn -oA host -PE --reason`
    - Shows the reason for a host being marked as "alive".

7. **Discovering Open TCP Ports**: `sudo nmap 10.129.2.28 --top-ports=10`
    - Scans for the top 10 TCP ports.

8. **Trace the Packets**: `sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping`
    - Traces packets sent and received during the scan.

9. **Connect Scan on TCP Port 443**: `nmap 10.129.2.28 -p 443 --packet-trace --disable-arp-ping -Pn -n --reason -sT`
    - Performs a TCP connect scan on port 443, showing all packets sent and received.

10. **Discovering Open UDP Ports**: `sudo nmap 10.129.2.28 -F -sU`
    - Performs a UDP scan on the top 100 ports.

11. **Version Scan**: `sudo nmap 10.129.2.28 -Pn -n --disable-arp-ping --packet-trace -p 445 --reason -sV`
    - Performs a service version detection scan.

12. **Service Enumeration**: `sudo nmap 10.129.2.28 -p- -sV`
    - Scans all ports for service version detection.

13. **Using Default Scripts**: `sudo nmap <target> -sC`
    - Executes default NSE scripts against the target.

14. **Specifying Scripts**: `sudo nmap <target> --script <category>`
    - Runs scripts from a specific category.

15. **Vulnerability Assessment**: `sudo nmap 10.129.2.28 -p 80 -sV --script vuln`
    - Uses vulnerability scripts to scan the target.

16. **Optimizing Scans with Timing Templates**: `sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms`
    - Adjusts the timing for scans to optimize performance.

17. **Decoy Scan**: `sudo nmap 10.129.2.28 -p 80 -sS -Pn -n --disable-arp-ping --packet-trace -D RND:5`
    - Uses decoys to mask the scanner's IP address.

18. **Testing Firewall Rule with OS Detection**: `sudo nmap 10.129.2.28 -n -Pn -p 445 -O`
    - Detects the operating system while scanning a specified port.


