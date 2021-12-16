# JSON Web Tokens
Tags: #jwt #session #token #jws #encryption
Related to:
See also: [[OAuth]][[authentication-sessions]]
Previous:

## Links

## Notes
### Important things to know
- JWT is used a lot by people who love full-stack JavaScript (e.g. MEAN/MERN stack)
- JWT is supposed to be stateless 
	--> A JWT cannot be "invalidated" in the same way a normal session token can be. A dev would need to build an ad-hoc system to block known JWTs.
	--> A sort of workaround for this is to use the `exp` field in JWTs and reject tokens that have a date older than e.g. 60 minutes ago.
- Often time a JWT will function as a session token
- Developers often put JWTs in localStorage or sessionStorage. It should not go there if it's sensitive! 
	--> localStorage and sessionStorage are always accessible to JavaScript
	--> Therefore if used to do session, it should be used as a Cookie. Then you can use the cookie security attributes (Secure, SameSite, HTTPOnly) to protect the info.
- JWTs can be (should be) signed using a hashing algorithm in order to prevent forgery. It is possible to switch the algorithm to `none` in some cases to defeat this

### Security Issues
#### Algorithm attacks
Change from default algorithm to `none`
	--> Burp JOSEPH can be used for this purpose

Symmetric key swap

#### Replay attacks
Find an old token somewhere in an app (or in device storage on mobile!) and use it as a session token by placing it in the correct cookie/localStorage spot