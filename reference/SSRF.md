# SSRF
Tags: #ssrf #api
Related to:
See also:
Previous:

## Links
Title:
Link:
About:

## Notes
- Anytime you can modify a URL that is passed server-side

### SSRF in APIs
_example from Portswigger: https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost_

A call to an API that checks the stock of a product in a store:

```POST /product/stock
[...]

stockApi=somewebsite.com:8080/checkStock
```

The value here could be changed to `localhost/admin` in order to access an administrator panel.