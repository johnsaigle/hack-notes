# Windows Command Cheatsheet
Tags: #activedirectory #powershell
Related to:
See also:
Previous:

## Enum
Show logged-in users
`Get-NetLoggedon -ComputerName <ip>`

Ask DC for information about user
`net user <username> /domain`

Show all Domain Admins
`net group "Domain Admins" /domain`

Show all users with Kerberos pre-auth disabled (AS-REP roast)
`Get-DomainUser -PreauthNotRequired -verbose`

Installed patches
`Get-Hotfix`

## Utility
Base64 decode
`[System.Text.Encoding]::Ascii.GetString([System.Convert]::FromBase64String($input))`

