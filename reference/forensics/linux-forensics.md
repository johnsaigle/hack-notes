# Linux Forensics

## When you have no forensics tools

### Faster

`cd /proc/<pid>; cp exe /tmp/outfile`

### Slower

- Use `ps -ef` to find process
- `cd /proc/<pid>`
- `cat maps`, note memory range in left column (given in hex)
- `dd if=mem bs=1 skip=$((<mem-range-start>)) count=$((<difference-between-mem-range-start-and-end>)) of=/tmp/outfile`
- Now the binary should be in `/tmp/outfile`

_Note: the `$(())` syntax evaluates hex to decimal._


## Links

https://www.youtube.com/watch?v=uYWTfWV3dQI
