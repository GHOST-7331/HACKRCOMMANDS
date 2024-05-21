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
| `ffuf -w ./folders.txt:FOLDERS,./wordlist.txt:WORDLIST,./extensions.txt:EXTENSIONS -u http://www.target.domain/FOLDERS/WORDLISTEXTENSIONS`         |                                                                                            |



