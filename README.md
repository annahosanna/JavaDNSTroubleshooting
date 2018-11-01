### Java DNS Troubleshooting Notes
Notes on troubleshooting Java hybrid cloud DNS issues

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
  * Add example Unbound config
  
  
