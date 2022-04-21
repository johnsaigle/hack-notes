# Pivoting
Tags: #ssh 

## SSH tunnel/port-forward
`ssh -L <local-ip>:<another-ip-reachable-by-remote-host>:<remote-port> <username>@<remote-host>`

e.g. From TryHackMe "Internal" box
`ssh -L 8888:172.17.0.2:8080 aubreanna@internal.thm`

## Copying a file from Linux to Windows
Must have an SSH server running on Windows:
`scp /path/to/file <IP>:C:/Users/user/dir`