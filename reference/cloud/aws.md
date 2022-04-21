# Amazon Web Services (AWS)
Tags:
Related to:
See also:
Previous:

## Links
Link: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
About: Installing AWS CLI tools

About: AWS Pentesting cheatsheet, including SSRF payloads discussed below
Link: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Cloud%20-%20AWS%20Pentest.md

## Notes

### Regions
> [AWS divides its infrastructure into Regions](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-available-regions), mostly independent clusters of datacenters. Within each region are availability zones (AZ). Each AZ in a region leverages separate power grids and usually are located in different flood plains. This redundancy allows you to establish highly resilient architectures to withstand significant weather or geological events, or more frequently, hardware or facility failures. 
	Because regions are independent - you'll get different answers to questions depending on the region you are querying. You can specify a region with the `--region` option to the AWS CLI.

### Attacks

#### Retrieving metadata
A successful request ([[ssrf]]) to `http://169.254.169.254/latest/meta-data/` will reveal metadata about an ec2 instance. It is possible to even pull out credentials using this kind of attack.