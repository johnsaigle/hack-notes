# CSRF
Tags:
Related to:
See also:
Previous:

## Links
Title:
Link:
About:

## Notes
Basic idea is to mimick a form on a target page and trick the user's browser into submitting it
The form would be something sensitive, like changing the email address associated with an account.

### Methodology
- Grab the important values from a `form` HTML element
- Host a server and create an HTML page with only the form in it.
- Fill out the form so that it POSTs to the target website.
- Prepopulate the values with the data you want, e.g. a new email for a targeted user account
- Deliver the link to your exploit server to the target in some way
- The user must be logged in for this to work

e.g. 
```
<form method='POST' action='https://aca91f1f1e6fd719c0f91c71004000b6.web-security-academy.net/my-account/change-email' id="csrf-form">
  <input type="email" name="email" value="hack@hacked.com">
  <input type='submit' value='submit'>
</form>
<script>document.getElementById("csrf-form").submit()</script>
```

### Mitigation
- Server-side, generate a unique token each time the form is loaded. The token should be tied to the user's session in some way
- Put this token into the form as a value
- When a POST is submitted, check that the token sent in the form matches what you generated on the server and came from the right user session
- Modern web app frameworks do this automatically for the most part

### Exploit/Bypass tips
- Sometimes servers will try to block POST requests from other domains and do checks for CSRF token. Changing the request from POST to GET might bypass it
- Try deleting a CSRF token entirely. A server might validate the token if present but not enforce it to be present in the first place.
- The CSRF token might not be unique or tied to a user session; bad designs will use a "pool" of tokens and rotate through them. So you might be able to reuse old tokens.