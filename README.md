### Java DNS Troubleshooting Notes
#### TL;DR
* Java applications are more likely to have DNS lookup errors than other applications if there is an over burdended DNS server.
* Java has short positive and negative TTL for entries in its DNS cache.
* Frequent lookups results in more burden on the DNS server.
* Frequent lookups increase the sample rate of detecting DNS errors, and thus report more errors.
* The Java DNS cache does not use the system DNS cache.
* The system as a whole and other applications that use the system DNS cache may not report any errors because system DNS caches, cache for a longer period of time - or at least respect the TTL of the record.
* A network with many JVMs puts more load on a DNS server.
* A DNS proxy such as Unbound or NSCD can be configured to return the last good lookup for a record, if a lookup fails.

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
    
  **TODO**
  * Add notes on Windows resolver, Java resolver, Linux resolver.
  * Add notes on local DNS cache like Unbound which can serve records with expired ttl - and where DDNS takes place.
  * Add notes on pps limits and mtu which can affect DNS
  * Add link to AWS hybrid cloud paper
  * Add example Unbound config https://d1.awsstatic.com/whitepapers/hybrid-cloud-dns-options-for-vpc.pdf?did=wp_card&trk=wp_card
```
serve-expired option to serve old responses from the cache with a TTL of zero
```
  * Stackoverflow article regarding Java DNS cache inspection https://stackoverflow.com/questions/1835421/java-dns-cache-viewer
  
