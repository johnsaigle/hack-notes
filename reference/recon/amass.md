# amass owasp scanner
Tags: #recon #dns #bruteforce #amass #lua
Related to: 
See also: [[ffuf]]
Previous:

## Links
Topic: How to Use Amass Efficiently
Author: @jeff_foley NahamCon2020
Link: https://github.com/OWASP/Amass

Topic: Amass scripting language
Author: OWASP
Linkl: https://github.com/OWASP/Amass/blob/master/doc/scripting.md

## Notes
- The results of different scans feedback into each other. "Cyclic"
	- Can result in slow execution time
	- `timeout` flag can be used to stop this from running forever

- `config.ini` can be used to specify replace regular CLI flags

- Output directory linux: `.config/amass`

### Operation modes
`amass db` -- see data, pull data out, add more
`amass enum` -- vertical recon (more like dirb)
`amass intel` -- horizontal recon (more like fierce)

### Usage

#### Bruteforcing
Enumerates, prints source (where it was discovered, IP, bruteforces _recursively_
`amass enum -src -ip -brute -w <wordlist> -df domains.txt`

**Recursive bruteforcing**:
- default value is 1, meaning: it needs to see at least one additional level before it begins
- so it won't bruteforce `ftp.site.com` unless it sees `www.ftp.site.com` somewhere else. After it does, then it will recurse a level


#### Iterative scanning
Each subsequent scan, all previous domains will be auto-included the next time

Raw list of discovered hosts
`amass db -names -d owasp.org`
Summary of how many hosts, IP blocks
`amass db -summary -d owasp.org`

#### Visualization
`amass viz -d3 -d owasp.org`

### Improving speed
- By default uses like 8 high-speed DNS resolvers
- DNS resolvers or name resolver for target will eventually throttle you
- Supplying more resolvers and rotating them can increase speed a lot
	`sort -R resolvers.txt | tail -25 > 25resolv.txt`
- [[DNS#DNS Validator with bruteforcing enumeration]]

**Command**
`amass enum -rf 25resolvers.conf -max-dns-queries 20000`

### Scripting
- _75m into the video_
- Lua scripts
- `.ads` extensions
- needs `name` and `type` global vars at the top
- function `newname()` can be used to add new hosts to the amass results
- external commands can be called --> you can run any program and process its input
-