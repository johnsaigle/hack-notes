
# NoSQL Injection
Tags: #nosql #php #mongo
Related to: [[sqli]]
See also:
Previous:

## Links
Title:
Link:
About:

## Notes
### Mongo + PHP
PHP will coerce arrays passed in params into key-value pairs that can be used for NoSQL injection

Example payload. Tries to login as the admin user and changes the logic from matching 'password == "admin"' to returning the result when the password does not equal 'admin'

`POST /login
[...]

username=admin&password[$ne]=admin
`

Example: show all users with a role of guest:
`/search?username[$ne]=a&role=guest`