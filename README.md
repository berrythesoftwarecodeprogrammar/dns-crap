# dns crap
scripts and info related to dnscrypt and alternative dns systems

## script: dnscrapt
A script designed to make managing dnscrypt (and regular dns) easier for me on my CentOS servers.

Requirements:  
A properly installed [dnscrypt-proxy](http://dnscrypt.org/) (see the next section on installing that)

Install:
```sh
chmod +x dnscrapt
mv dnscrapt /usr/sbin/
```

Usage:  
`dnscrapt update` -- updates dnscrypt-resolvers.csv  
`dnscrapt list` -- lists the best dnscrypt resolvers. DNSSEC+NO\_LOGS+NAMECOIN come first and then DNSSEC+NO_LOGS  
`dnscrapt <dnscrypt-resolver> [<backup-dnscrypt-resolver>]` -- runs the dnscrypt-proxy service with the chosen resolver(s). max: 2  
`dnscrapt <opennic-au|opennic-ch|opennic-de|opennic-ec|opennic-fr|opennic-it|opennic-jp|opennic-ro|opennic-si|opennic-uk|opennic-us` -- switches to regular dns and uses opennic resolvers belonging to the specified region  
`dnscrapt <okturtles|level3|opendns>` -- switches to one of the regular dns providers listed  

Notes:  
I like to use `dnscrapt soltysiak-ipv6 soltysiak`, which makes `soltysiak-ipv6` the primary resolver and `soltysiak` as a backup (for when ipv6 is acting up). The non-dnscrypt options are available only as backups for when no dnscrypt resolvers are working (for whatever reason). `okturtles` would be the preferred non-dnscrypt backup option, with the `opennic` servers following. `level3` and `opendns` probably cant be trusted much but they work when nothing else does.  
I manually added the `opennic` servers from [opennicproject](https://servers.opennicproject.org/) and they can change any time. I also added in IPv6 servers and gave them priority, so it might cause problems on servers without IPv6. I got the `okTurtles` dnschain server from [dnschain github](https://github.com/okTurtles/dnschain/blob/master/docs/How-do-I-use-it.md#Servers). 

TODO:  
1. Add a way to scrape the opennic servers page and deliver best results  
2. Add an option to list ALL dnscrypt resolvers or search through them  
3. Put non-dnscrypt resolvers in a config file of some sort instead of hardcoding them, which also lets users add their own custom servers  

## installing dnscrypt on linux (centOS)
