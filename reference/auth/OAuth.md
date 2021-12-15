# OAuth hacking
Tags: #oauth #openid

## Links
- https://book.hacktricks.xyz/pentesting-web/oauth-to-account-takeover
- https://portswigger.net/web-security/oauth
- ippsec Oouch video: https://youtu.be/EUtqjK27MxQ

## Overview
OAuth authentication is generally implemented as follows:
1.  The user chooses the option to log in with their social media account. 
	1.  The client application then uses the social media site's OAuth service to request access to some data that it can use to identify the user. (e.g. email)
2.  After receiving an access token, the client application requests this data from the resource server, typically from a dedicated `/userinfo` endpoint.
3.  Once it has received the data, the client application uses it in place of a username to log the user in. The access token that it received from the authorization server is often used instead of a traditional password.

## Issue with `state`
`state` can tie a request to authorize an account with a 3rd-party login with an existing session on the original app.

`state` should (almost) always be present! Sort of works like a CSRF
> Note that if the site allows users to log in exclusively via OAuth, the `state` parameter is arguably less critical. However, not using a `state` parameter can still allow attackers to construct login CSRF attacks, whereby the user is tricked into logging in to the attacker's account.

e.g. if `site.com` gives me a sessionid `abcd`, and I authorize `socialnetwork.com` to let me into `site.com` using my `socialnetwork.com` creds, `site.com` can send `abcd` along with that authorization.
`socialnetwork.com` will send back a valid `code` used to tie the two sites together. 
`state` in this case means that the user account on `site.com` that is being linked via `code` to `socialnework.com` must be using `abcd` to make use of `code`

SO --> if there is no `state`, it is possible that `socialnetwork.com` will send `code` the code link, but another user executes it! (e.g. via CSRF, phishing, whatever.)
### Attack
Attacker generates an authorization with `socialnetwork.com` and feeds the `code` link to the target on `site.com`, then the attacker can use their `socialnetwork.com` creds to login _as the target_ to `site.com`. 

This whole process essentially short-circuits `site.com`'s authentication flow and joins the attacker's OAuth login to the target's account.


## Register a malicious app
_Workflow for `authorization_code` grant type_
Can register a new client_id, client_secret, etc. and redirect-uri. 

If you can feed a GET request for authorization to (to e.g. `/oauth/authorize` on a Django server) that app (containing the `client_id` and `redirect_uri` you made) to another user, OAuth will send a one-time `code` to `redirect_uri`. 

You control `redirect_uri` so you can see `code` -- and maybe a user's sessionid.
`code` at this point is already linked to a user's account on `site.com`. You can then feed `code` to e.g. `/oauth/token` on Django:

```POST /oauth/token
[...]

grant_type=authorization_token&code=XXX&client_id=YYY
```

The response will include an a token for Bearer, and a refresh token. Refresh token is a "master token" that can generate new auth/API tokens or whatever.

