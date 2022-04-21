# iOS Mobile Security
Tags:
Related to:
See also:
Previous:

## Links
Title: iOS Pentesting Checklist
Link: https://mobexler.com/checklist.htm#ios
About: List of things to check when auditing an iPhone app.

OWASP Mobile Security Testing Guide
Link: https://mobile-security.gitbook.io/mobile-security-testing-guide/
About: Free ebook from OWASP about mobile security

## Notes
_See mobexeler.com's resources for all of the below._

### Basics
- You must have a jailbroken device to test iOS apps
	- Literally the only other option is [Corellium](https://www.corellium.com/) which is a paid ARM emulation service
	- Keep in mind that the latest few minor versions of iOS will not usually have available jailbreaks. If you install an iOS version that cannot (currently) be jailbroken, it is extremely difficult and potentially impossible to rollback to an earlier version.
	- The oldest iOS device likely to be compatible and usable with modern apps is an iPhone 6. (These can be found used for around $60 CAD.)
- iOS apps come in `.ipa` files
- To get the `.ipa` app, you must decrypt it and dump it from memory on the device
- To install an `.ipa` you've found or modified, you must sign it using an "iOS app signer"
- The `.ipa` must be installed using a mac computer and XCode

### Finding domains an app connects to
Easy mode:
> New iOS feature useful to bug bounty hunters. iOS can now show you what domains an app connects to. There are better ways of course, but it's handy. Settings -> Privacy -> App Privacy Report
https://twitter.com/NathOnSecurity/status/1471569525263028225/photo/1
