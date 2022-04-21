# Oracle
Tags: #sqli #database
Related to:
See also:
Previous:

## Links
Oracle research DB, linked to by Naffy
http://www.davidlitchfield.com/security.htm

## Notes
### SELECTing
On Oracle, every `SELECT` query must use the `FROM` keyword and specify a valid table. There is a built-in table on Oracle called `dual` which can be used for this purpose. 
So the injected queries on Oracle would need to look like: `' UNION SELECT NULL FROM DUAL--`.

### Concatenate
`||`, e.g.
`' UNION SELECT username || '~' || password FROM users--`

### No stacked/batched queries
`UNION` must be used instead.