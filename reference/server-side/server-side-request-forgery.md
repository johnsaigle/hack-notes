# SSRF
Tags: #ssrf #api
Related to:
See also:
Previous:

## Links
Title:
Link:
About:

Title: Turning bad SSRF to good SSRF: Websphere Portal
Link: https://blog.assetnote.io/2021/12/26/chained-ssrf-websphere/
About: Case study about turning Open Redirect vulnerabilities into SSRF. References the well-known 2021 Grafana exploit. Also goes into detail about decompiling JAR files to find vulnerabilities in applications.

## Notes
- Anytime you can modify a URL that is sent to the server

### SSRF in APIs
_example from Portswigger: https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost_

A call to an API that checks the stock of a product in a store:

```POST /product/stock
[...]

stockApi=somewebsite.com:8080/checkStock
```

The value here could be changed to `localhost/admin` in order to access an administrator panel.