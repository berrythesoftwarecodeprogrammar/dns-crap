# dns crap
scripts and info related to dnscrypt and alternative dns systems

## root script: dnscrapt [🡇](https://raw.githubusercontent.com/berrythesoftwarecodeprogrammar/dns-crap/master/dnscrapt)
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
To have it run on boot just add your `dnscrapt` command to your start up scripts. I add the command to the file `/etc/rc.local`

TODO:  
1. Add a way to scrape the opennic servers page and deliver best results  
2. Add an option to list ALL dnscrypt resolvers or search through them  
3. Put non-dnscrypt resolvers in a config file of some sort instead of hardcoding them, which also lets users add their own custom servers  

## installing dnscrypt on linux (centOS)  
I don't think the instructions will be very different on other UNIX sytems. The instructions below download+install the latest version of the dependency `libsodium` and then download+install the latest version of `dnscrypt-proxy`. Then it downloads the latest `dnscrypt-resolvers.csv` and adds the system user `dnscrypt` for running the service.
```bash
wget https://download.libsodium.org/libsodium/releases/LATEST.tar.gz -O libsodium.tar.gz
tar -xzvf libsodium*.tar.gz
cd libsodium*
./configure
make && make install
echo /usr/local/lib > /etc/ld.so.conf.d/usr_local_lib.conf
ldconfig
cd ..
rm -rf libsodium*

wget https://download.dnscrypt.org/dnscrypt-proxy/LATEST.tar.gz -O dnscrypt-proxy.tar.gz
tar -xzvf dnscrypt*.tar.gz
cd dnscrypt*
./configure
make && make install
cd ..
rm -rf dnscrypt*

wget https://raw.githubusercontent.com/jedisct1/dnscrypt-proxy/master/dnscrypt-resolvers.csv -O /usr/local/share/dnscrypt-proxy/dnscrypt-resolvers.csv

groupadd dnscrypt
useradd -r -g dnscrypt -s /sbin/nologin dnscrypt -d /var/lib/dnscrypt
mkdir /var/lib/dnscrypt
chown -R dnscrypt.dnscrypt /var/lib/dnscrypt
chmod -R 600 /var/lib/dnscrypt
```
That's it! Now use my `dnscrapt` script which is above or run the proxy manually and then edit your system dns settings. CentOS example:  
```
dnscrypt-proxy -E -R cloudns-syd -d -u dnscrypt
echo "nameserver 127.0.0.1" > /etc/resolv.conf
```
or you could run another one as a backup resolver, on another port, and specify the ports in your `resolv.conf` like this:
```
dnscrypt-proxy -a 127.0.0.1:53 -E -R cloudns-syd -d -u dnscrypt
dnscrypt-proxy -a 127.0.0.1:54 -E -R cloudns-can -d -u dnscrypt
echo "nameserver 127.0.0.1#53" > /etc/resolv.conf
echo "nameserver 127.0.0.1#54" >> /etc/resolv.conf
```
To have it run on boot just add the commands you use to run it in your start up scripts. I add the commands to the file `/etc/rc.local`

## dnscrypt resolvers list

I'm working on a page [https://berr.yt/h/dnscrypt-resolvers/](https://berr.yt/h/dnscrypt-resolvers/) which aims to make it easier to view `dnscrypt-resolvers.csv` since there is currently no good way to do so. It's a WIP.

TODO:  
1. sorting of columns  

## sources and further reading
- [http://dnscrypt.org/](http://dnscrypt.org/)  
- [https://github.com/jedisct1/dnscrypt-proxy](https://github.com/jedisct1/dnscrypt-proxy)
- [https://github.com/okTurtles/dnschain](https://github.com/okTurtles/dnschain)  
- [https://namecoin.info/](https://namecoin.info/)  
- [http://emercoin.com/EMCDNS_and_NVS](http://emercoin.com/EMCDNS_and_NVS)  
- [https://www.opennicproject.org/](https://www.opennicproject.org/)  

## TODO
1. Add my windows version of dnscrapt  
2. Talk about alt dns like opennic and namecoin/dnschain  
