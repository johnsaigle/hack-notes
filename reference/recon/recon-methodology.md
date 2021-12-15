# Recon Methodology
Tags: #recon #amass #asn #dns #subdomain #nuclei
Related to:
See also:
Previous:

## Links
Title: Bug Hunter's Nethodology v4.0
Author: jhaddix
Link: https://youtu.be/p4JgIu1mceI?list=PLJgAGZ8VkieiHqrJqMjQZ49jLdMkG5cIF

## Notes
### Roots/Seeds
#### Research acquisitions 
Acquisitions the company has been involved in
--> crunchbase.com

#### Do ASN enumeration
- Given to large enough networks of an organization
http://bgp.he.net
- metabigor
- asnlookup
- Can use [[amass]] with -asn switch

#### Reverse WHOIS
-Look up a company, see what is registed to them
- one example: whoxy.com
	Free API to get credits for auto
	- CLI tool: DOMLink
		- recursive
- Verify. Sometimes data isn't all that accurage

#### Ad/Analytics relationships
- BuiltWith (also there's a browser extension)
- Relationships tab
- Tracker codes, usually consistent across all their domains

#### Legal dorks
- Copyright, ToS agreements, privacy policies

#### Shodan
- e.g. searching on Twitch yields --> twitch.amazon.eu

### Subdomains
#### Subdomain link-finding
- GoSpider (does JS parsing well)
	- not-recursive
- hakrawler (good bug hunting parsers)
	https://github.com/hakluke/hakrawler
- SubDomainizer
	- Anayses JS for subdomains, cloud services, and Shannon ENtropy to find sensitive info in JS files
	- not recursive
- subscraper
	- scrapes subdomains
	- recursive
	
#### DNS info sites
DNS dumpster, censys, netcraft, security trails

#### Google dorks
`site:root.com -sub1.root.com ...`
- Do this iteratively

#### DNS Scraping tools
[[amass]]
- A lot of the DNS info sites as sources, can add more
- Can feed the ASN summary back into `intel` mode

subfinder

github-subdomains.py
- part of `github-search` suite
- good to add sleeps and rate-limiting, as well as running the script many times, due to weird GitHub API behaviour

shosubgo
- Shodan scraper in go
- Apparently it's dope

#### Cloud Range scraping
- Scrape whole ranges of GCP, AWS, or Azure space and look in SSL certs for something related to the target
- Could maybe build this using massscan
- https://www.daehee.com/scan-aws-ip-ssl-certificates/

### DNS bruteforcing
- bruteforcing should be done different DNS servers and do so in parallel. Speeds things up a ton
- amass 
	- amass -- `-rf` flag adds more resolvers [[amass#Improving speed]]
	- amass also allows us to resolve the domains (vs other bruting tools)
- shuffledns is another option

#### wordlists
Big: jhaddix all.txt
Specialized: 
	- `cewl`,
	- custom HTML parsing
	- commonspeak2

#### Alternations/permutations
- e.g. dev1 --> dev2, dev-1, etc.
- included in amass by default

### Other
#### Port Analysis
masscan --> can out perform nmap in big cases
- only scans on IPs, not domains
- First run masscan to find open stuff, then go to nmap
`masscan -p1-65535 -iL <ips> --max-rate 1800 -oG outputfile.log`
- https://danielmiessler.com/study/masscan/

dnmasscan can resolve the domains for you and go into masscan
- or you can write your own

#### Service scanning
brutespray
- send nmap/masscan -oG output file to the tool and try a bunch of default credentials

#### Manual github dorking
See jhaddix list
- https://gist.github.com/jhaddix/77253cea49bf4bd4bfd5d384a37ce7a4

#### Screenshotting
e.g. aquatone

#### Subdomain takeover
https://github.com/edoverflow/can-i-take-over-xyz
Can automate discovery of this using nuclei rules

### Automation
`interlace` can be used to wrap around tools to make them threaded and
accept different kinds of inputs, like CIDR ranges

Links:
https://github.com/codingo/Interlace
https://hakluke.medium.com/interlace-a-productivity-tool-for-pentesters-and-bug-hunters-automate-and-multithread-your-d18c81371d3d
https://github.com/codingo/Interlace