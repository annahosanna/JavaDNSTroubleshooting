### Java DNS Troubleshooting Notes
#### TL;DR
* Java applications are more likely to have DNS lookup errors than other applications if there is an over burdended DNS server.
* Java has short positive and negative TTL for entries in its DNS cache (10 seconds), and it does not respect the DNS record's TTL.
* Frequent lookups results in more burden on the DNS server. (thus more DNS servers maybe needed to distribute the load)
* Frequent lookups increase the sample rate of detecting DNS errors (DNS server failure to respond or respond in a timely manner), and thus report more errors, and thus more Exceptions.
* The Java DNS cache does not use the system DNS cache.
* The system as a whole may not report any errors because it respects the TTL of the record and thus different cache expiration duration.
* A network with many JVMs (i.e. Java applications) puts more load on DNS servers.
* A DNS proxy such as Unbound or NSCD can be configured to return the last good lookup for a record, if a lookup fails.
* This issue has been observed in hybrid networks with Infoblox running in the cloud. Scaling up or out Infoblox reduces the issue.

#### Notes on troubleshooting Java hybrid cloud DNS issues

Basic functionality tested
```
InetAddress[] results = java.net.InetAddress.getAllByName(hostname);
```

* Various JRE and java.security flags that affect DNS and DNS providers
  1. DNS providers and the order they are evaluated in
    	* `sun.net.spi.nameservice.provider.1`
    	* Values such as 'sun,dns'
  2. Oracle specific JRE options for DNS ttl (Do not use these):
	* `sun.net.inetaddr.ttl`
	* `sun.net.inetaddr.negative.ttl`
  3. JRE security properties for DNS ttl (Use these):
    	* `networkaddress.cache.ttl`
    	* `networkaddress.cache.negative.ttl`
    
```
server:
  serve-expired: yes
  # 900 seconds is 15 minutes which is reasonable, but Boto3 endpoints have a DNS timeout of 60 seconds.
  serve-expired-ttl: 60            
  # 1.8 seconds, in milliseconds
  serve-expired-client-timeout: 1800  
```

* Stackoverflow article regarding Java DNS cache inspection https://stackoverflow.com/questions/1835421/java-dns-cache-viewer
* AWS paper regarding DNS configuration [AWS paper link](https://docs.aws.amazon.com/whitepapers/latest/hybrid-cloud-dns-options-for-vpc/additional-considerations.html)
* [Packet Per Second DNS limits for Route53](https://repost.aws/knowledge-center/vpc-find-cause-of-failed-dns-queries) 
