# Password Reset
Tags:
Related to:
See also:
Previous:

## Links
Title:
Link:
About:

## Notes

### Burp trick for takeover
- Create your own account on target with real email
- Issue password reset
- This will typically send a token to your email
- Open the email, copy the token value
- Search for the token in Burp. 
- If it appears at all --> account takeover
