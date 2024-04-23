# Infrastructure-based Enumeration

## Certificate Transparency
| **Command** | **Description** |
|-------------|-----------------|
| `curl -s https://crt.sh/\?q\=<target-domain>\&output\=json \| jq .` | Fetches certificate transparency logs for the specified domain. |

## Shodan Scans
| **Command** | **Description** |
|-------------|-----------------|
| `for i in $(cat ip-addresses.txt); do shodan host $i; done` | Scan each IP address in a list using Shodan. |

---

# Host-based Enumeration

## FTP (Port 21)
| **Command** | **Description** |
|-------------|-----------------|
| `ftp <FQDN/IP>` | Interact with the FTP service on the target. |
| `nc -nv <FQDN/IP> 21` | Netcat connection to interact with the FTP service on the target. |
| `telnet <FQDN/IP> 21` | Telnet to interact with the FTP service on the target. |
| `openssl s_client -connect <FQDN/IP>:21 -starttls ftp` | Secure connection to interact with FTP service. |
| `wget -m --no-passive ftp://anonymous:anonymous@<target>` | Download all available files from the FTP server using wget. |

## SMB (Port 445)
| **Command** | **Description** |
|-------------|-----------------|
| `smbclient -N -L //<FQDN/IP>` | Null session authentication on SMB. |
| `smbclient //<FQDN/IP>/<share>` | Connect to a specific SMB share. |
| `rpcclient -U "" <FQDN/IP>` | Interaction with the target using RPC. |
| `samrdump.py <FQDN/IP>` | Username enumeration using Impacket scripts. |
| `smbmap -H <FQDN/IP>` | Enumerating SMB shares. |
| `crackmapexec smb <FQDN/IP> --shares -u '' -p ''` | Enumerating SMB shares using null session authentication. |
| `enum4linux-ng.py <FQDN/IP> -A` | Comprehensive SMB enumeration. |

## NFS (Ports 2049, 111)
| **Command** | **Description** |
|-------------|-----------------|
| `showmount -e <FQDN/IP>` | Show available NFS shares. |
| `mount -t nfs <FQDN/IP>:/<share> ./target-NFS/ -o nolock` | Mount the specific NFS share. |
| `umount ./target-NFS` | Unmount the specific NFS share. |

## DNS (Port 53)
| **Command** | **Description** |
|-------------|-----------------|
| `dig ns <domain.tld> @<nameserver>` | NS request to the specific nameserver. |
| `dig any <domain.tld> @<nameserver>` | ANY request to the specific nameserver. |
| `dig axfr <domain.tld> @<nameserver>` | AXFR request for DNS zone transfer. |
| `dnsenum --dnsserver <nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list <domain.tld>` | Subdomain brute forcing using dnsenum. |

## SMTP (Port 25)
| **Command** | **Description** |
|-------------|-----------------|
| `telnet <FQDN/IP> 25` | Connect and interact with the SMTP service via Telnet. |
| `nmap -sC -sV -p25 {target}` | Nmap scan on SMTP port to enumerate service details and vulnerabilities. |
| `nmap -p 25 --script smtp-brute` | Perform brute force attack on SMTP service using nmap scripts. |
| `smtp-user-enum -M VRFY -U footprinting-wordlist.txt -t <FQDN/IP>` | Enumerate SMTP users using the VRFY command. |
| `msf6 auxiliary(scanner/smtp/smtp_enum)` | Use Metasploit to enumerate SMTP users. |

## IMAP/POP3 (Ports 143, 110, 993, 995)
| **Command** | **Description** |
|-------------|-----------------|
| `curl -k 'imaps://<FQDN/IP>' --user <user>:<password>` | Log in to the IMAPS service using cURL. |
| `openssl s_client -connect <FQDN/IP>:imaps` | Connect to the IMAPS service using OpenSSL. |
| `openssl s_client -connect <FQDN/IP>:pop3s` | Connect to the POP3s service using OpenSSL. |
| `nmap <FQDN/IP> -sV -p110,143,993,995 -sC` | Enumerate IMAP services using nmap with service version detection and default scripts. |
| `a login <username> <password>` | IMAP command to log in. |
| `a LIST "" "*"` | IMAP command to list all mailboxes. |
| `a SELECT "DEV.DEPARTMENT"` | IMAP command to select a specific mailbox. |
| `a FETCH 1 BODY[]` | IMAP command to fetch contents of the first email in the mailbox. |

## SNMP (Port 161)
| **Command** | **Description** |
|-------------|-----------------|
| `snmpwalk -v2c -c <community string> <FQDN/IP>` | Walk through SNMP OIDs using the community string. |
| `onesixtyone -c community-strings.list <FQDN/IP>` | Brute force SNMP community strings. |
| `braa <community string>@<FQDN/IP>:.1.*` | A mass SNMP scanner. |

## MySQL (Port 3306)
| **Command** | **Description** |
|-------------|-----------------|
| `mysql -u <user> -p<password> -h <FQDN/IP>` | Login to the MySQL server. |
| `sudo nmap <FQDN/IP> -sV -sC -p3306 --script mysql*` | Nmap scan on MySQL port to enumerate service details and vulnerabilities. |

# MySQL Commands Cheat Sheet

This cheat sheet provides a comprehensive list of basic commands for interacting with a MySQL database server. It includes instructions for connecting to the server, viewing databases, and manipulating database contents.

```bash
# Connect to MySQL Server:
mysql -u <user> -p<password> -h <IP address>
# Note: There should not be a space between the '-p' flag and the password.

# Show All Databases:
show databases;
# Displays all databases available on the server.

# Select a Database:
use <database>;
# Selects one of the existing databases to interact with.

# Show All Tables:
show tables;
# Displays all tables within the selected database.

# Show All Columns in a Table:
show columns from <table>;
# Shows all columns in the selected table.

# Show All Rows in a Table:
select * from <table>;
# Displays all data within the specified table.

# Search Within a Table:
select * from <table> where <column> = "<string>";
# Search for a specific string within a column in the desired table.


## MSSQL (Port 1433)
| **Command** | **Description** |
|-------------|-----------------|
| `mssqlclient.py <user>@<FQDN/IP> -windows-auth` | Log in to the MSSQL server using Windows authentication. |

## IPMI (Port 623)
| **Command** | **Description** |
|-------------|-----------------|
| `msf6 auxiliary(scanner/ipmi/ipmi_version)` | IPMI version detection using Metasploit. |
| `msf6 auxiliary(scanner/ipmi/ipmi_dumphashes)` | Dump IPMI hashes using Metasploit. |

## Linux Remote Management
| **Command** | **Description** |
|-------------|-----------------|
| `ssh-audit.py <FQDN/IP>` | Remote security audit against the target SSH service. |
| `ssh <user>@<FQDN/IP>` | Log in to the SSH server using the SSH client. |
| `ssh -i private.key <user>@<FQDN/IP>` | Log in to the SSH server using a private key. |
| `ssh <user>@<FQDN/IP> -o PreferredAuthentications=password` | Enforce password-based authentication during SSH login. |

## Windows Remote Management
| **Command** | **Description** |
|-------------|-----------------|
| `rdp-sec-check.pl <FQDN/IP>` | Check the security settings of the RDP service. |
| `xfreerdp /u:<user> /p:"<password>" /v:<FQDN/IP>` | Log in to the RDP server from Linux using FreeRDP. |
| `evil-winrm -i <FQDN/IP> -u <user> -p <password>` | Log in to the WinRM server using Evil-WinRM. |
| `wmiexec.py <user>:"<password>"@<FQDN/IP> "<system command>"` | Execute system commands on a Windows machine over WMI. |

## Oracle TNS (Port 1521)
| **Command** | **Description** |
|-------------|-----------------|
| `./odat.py all -s <FQDN/IP>` | Comprehensive scanning of Oracle database services using ODAT. |
| `sqlplus <user>/<pass>@<FQDN/IP>/<db>` | Log into the Oracle database using SQL*Plus. |
| `./odat.py utlfile -s <FQDN/IP> -d <db> -U <user> -P <pass> --sysdba --putFile C:\\insert\\path file.txt ./file.txt` | Use ODAT to upload a file to the server using the UTL_FILE package. |
