# PHP Deserialization
Tags: #php #deserialization #fileupload #waf #bypass 
Related to:
See also:
Previous:

## Links
TItle: Advanced PHP Deserialization
Topic: ippsec explaining PHAR attacks
Link: https://www.youtube.com/watch?v=fHZKSCMWqF4

Title: PHPGGC: PHP Generic Gadget Chains
Topic: Payloads for PHP deserialization chains in a variety of big frameworks
Link: https://github.com/ambionics/phpggc

## Notes
### Basic
- Exploit goes in e.g. `__construct()` function
 
### Phar format attacks
- Serialized zipped format
- Can bypass extension/mime inspection
- `phar://` triggers `unserialize()`
	- POP chain must start with wakeup or destruct
		- So you're attacking class that uses one of these methods
	- You want to feed a `phar://some-file` payload to a file function.
		- e.g: `file_put_contents`, `md5_file`, many more
	
#### Attack code
[[php-POPchain.png]]
- You must set `phar.readonly` to Off in php.ini to run the Attack ccode


Attack steps:
- Generate attack code in PHP phar generator
- Make the phar file
- Use file uploader to upload phar file
- Use file delete function of file upload to execute code that is in the `__contruct()` method of the phar object
#### Bypassing filters
Can add e.g. `GIF8;` in front of `->setStub` to trick a GIF filter
You could instead literally place a whole image there by doing:
- `$image = file_get_contents('img.png');`
- And then prepend `$image` in `->setStub`

	

