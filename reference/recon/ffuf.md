# FFUF
Tags: #recon #bruteforce #wordlist #idor #go
Related to:
See also: [[wordlists]]
Previous:

## Links
https://tryhackme.com/room/ffuf

## Fuzzing endpoints
### See only a specific status code using -mc
`ffuf -u http://10.10.251.31/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -mc 200`

### Exclude status codes using -fc
`ffuf -u http://10.10.251.31/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -fc 401,403`

### Exclude files with a specific size with -fs
`ffuf -u http://10.10.251.31/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -fs 200`

### Filter using regex, e.g. files starting with '.'
`ffuf -u http://10.10.251.31/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -fr '/\..*'`

### Finding out valid extensions, using e.g. index as a prefix
`ffuf -u http://10.10.251.31/indexFUZZ -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt`


### Adding known extensions to the end of common words
`ffuf -u http://10.10.251.31/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt -e .php,.txt`

## Fuzzing parameters

`ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?FUZZ=1' -c -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -fw 39`

## POSTing data
`ffuf -u http://10.10.102.18/sqli-labs/Less-11/ -c -w /usr/share/seclists/Passwords/Leaked-Databases/hak5.txt -X POST -d 'uname=Dummy&passwd=FUZZ&submit=Submit' -fs 1435 -H 'Content-Type: application/x-www-form-urlencoded'`

## Subdomains and vhosts
Subdomain example:
`ffuf -u http://FUZZ.mydomain.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -fs 0`

Vhost example:
`ffuf -u http://mydomain.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H 'Host: FUZZ.mydomain.com' -fs 0`

## Proxy
Normal proxy
`ffuf -u http://10.10.102.18/ -c -w /usr/share/seclists/Discovery/Web-Content/common.txt -x http://127.0.0.1:8080`

Only pass matches to proxy:
`ffuf -u http://10.10.102.18/ -c -w /usr/share/seclists/Discovery/Web-Content/common.txt -replay-proxy http://127.0.0.1:8080`