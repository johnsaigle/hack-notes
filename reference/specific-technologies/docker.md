# Docker 
Tags: #docker #privesc #techniques #capsh #capabilities #nc #netcat 
Author: cmnatic (TryHackMe), https://tryhackme.com/room/dockerrodeo
Related to: [[linux-privesc]], [[shells]]

## Notes
### General
Services (e.g. web apps) in Docker containers are configured to run as `root` user by default.

### Detection
Use `ps aux`. If there are very few processes, likely a container.

Look for `/.dockerenv`

`cd /proc/1/`, `cat cgroup`. Some files might contain the string "docker"

### Attacks

#### Examining images in a registry
- Can pull info from a registry
- Can view manifest: shows commands run, sometimes creds on command-line

#### Rev eng image
- Dive tool https://github.com/wagoodman/dive
- Download image, run dive using the Image id, examine layers and find secrets. Also can see the state of the underlying OS

#### Malicious docker files
- Can make a simple docker image on a slim linux build
- Get it to install e..g netcat
- When run, create a reverse shell back to you
- Ideas in [[shells]]

#### Finding exposed port
- Docker can be exposed to Internet for remote management
- Port: 2357
- `docker -H '...' <regular command>`

### Priv esc
#### Mounting root fs in new container
- Requires user is a part of `docker` group (ie. can run commands)
- Mounts the host directory to a new container and then connect to that to reveal all the data on the host OS
`docker run -v /:/mnt --rm -it alpine chroot /mnt sh`

#### Changing process namespace
As root in Docker container, migrate to namespace of process 1 (OS boot process)
- `nsenter --target 1 --mount sh`

Gives root on underlying OS

#### Privileged containers
- Special mode of execution where Docker containers can access root FS (bypassing the Docker Engine)
- Use `capsh --print` to show Linux capabilities --> reveals what you can do
	- e.g. grep for `sys_admin`
	- See also: [[linux-privesc#Linux capabilities]]
- See references [[#References]] for example container escape PoC using `cgroups`


## References
[Understanding Docker container escapes](https://blog.trailofbits.com/2019/07/19/understanding-docker-container-escapes/#:~:text=The%20SYS_ADMIN%20capability%20allows%20a,security%20risks%20of%20doing%20so)
[Docker commands cheatsheet](https://raw.githubusercontent.com/sangam14/dockercheatsheets/master/dockercheatsheet8.png)