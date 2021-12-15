# Windows Priviliege escalation
Tags: #powershell #privesc
Related to:
See also:
Previous:

## Links
WinPEAS
Link: https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS/winPEASexe/binaries

Things to look at:

SAM file which contains passwords
`%SystemRoot%\System32\config\SAM\`

## Methods
Using something like [PowerUp.ps1](https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1), you can identify privesc methods

### Restarting Services
If PowerUp marks something as `CanRestart` to be true, it means you can restart a service using `Start-Service`, `Stop-Service`, or `Restart-Service`.

I think this works only if the service is configured to run with root-level access.

If you also have permissions to delete the associated binary, you can replace it with e.g. a reverse shell.

Powershell Steps:
- `Stop-Service 'XXX'`
- Start handler (nc or metasploit)
- Replace binary
- `Start-Service 'XXX'`

CMD:
- `sc stop XXX`
- `sc start XXX`