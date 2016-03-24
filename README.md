# dns crap
scripts and info related to dnscrypt and alternative dns systems

## script: dnscrapt
A script designed to make managing dnscrypt (and regular dns) easier for me on my CentOS servers.

Install:
```sh
chmod +x dnscrapt
mv dnscrapt /usr/sbin/
```

Usage:  
`dnscrapt update` updates dnscrypt-resolvers.csv  
`dnscrapt list` lists the best dnscrypt resolvers. DNSSEC+NO_LOGS+NAMECOIN come first and then DNSSEC+NO_LOGS  
`dnscrapt <dnscrypt-resolver> [<backup-dnscrypt-resolver>]` runs the dnscrypt-proxy service with the chosen resolver(s). max: 2  
`dnscrapt <opennic-au|opennic-ch|opennic-de|opennic-ec|opennic-fr|opennic-it|opennic-jp|opennic-ro|opennic-si|opennic-uk|opennic-us` switches to regular dns and uses opennic resolvers belonging to the specified region  
`dnscrapt <okturtles|level3|opendns>` switches to one of the regular dns providers listed  

## installing dnscrypt on linux (centOS)
