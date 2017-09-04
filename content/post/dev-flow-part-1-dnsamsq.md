+++
Description = ""
tags = ["development", "devops", "docker", "dnsmasq"]
date = "2015-06-04T21:38:15-07:00"
title = "A Prodlike dev flow part 1 - dnsmasq "
author = "moofish32"

+++

### DNS for development just like your production setup

dnsmasq is one part of the equation to making our development environments behave like a production
environment.  Instead of coding in URLs like http://localhost:8080/ we want to provide
http://testing.coolwebsite.dev/.  This way we can have websites hosted on subdomains where we browse
and function locally, the same as you will with a production environment.  In addition, we are
planning to use [Consul](https://www.consul.io) for service discovery which leverages DNS. This post
solves the DNS problem associated with this flow.  In order to completely get the
[dev-prod](http://12factor.net/dev-prod-parity) parity, additional tools will be necessary, such as
load balancers or reverse proxies (think nginx and watch for a follow up post to elaborate).


### The Setup

```
brew up
brew install dnsmasq
cp /usr/local/opt/dnsmasq/dnsmasq.conf.example /usr/local/etc/dnsmasq.conf
sudo cp -fv /usr/local/opt/dnsmasq/*.plist /Library/LaunchDaemons
sudo chown root /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
```
At this point you now a have locally running dns server.  Let's go configure it so that we can browse like we are in a production environment.

We need to tell our DNS server where to lookup hostnames for our development environment.  I
typically use a domain name with a .dev top level domain.  For instance when working on http://www.client.com my test domain is client.dev.  This is configured by adding the name address pair as follows: 
```
sed -i.bak '/#address=\/double-click\.net\/127\.0\.0\.1/a\
  address=/client.dev/192.168.59.103\' /usr/local/etc/dnsmasq.conf
```
The whitespace in this command for OSX must match exactly or you will discover the GNU vs BSD sed differences. As an alternative you can just open /usr/local/etc/dnsmasq.conf in your favorite editor and append this line.

This setup is preparing us to pair with [Docker Compose](https://docs.docker.com/compose/). If you are not going to use Docker Compose, then you probably just want to use the loopback address as below:
```
sed -i.bak '/#address=\/double-click\.net\/127\.0\.0\.1/a\
  address=/client.dev/127.0.0.1' /usr/local/etc/dnsmasq.conf
```
Finally in order to get these changes into dnsmasq we need to restart the server with: 
``` 
sudo launchctl stop homebrew.mxcl.dnsmasq
sudo launchctl start homebrew.mxcl.dnsmasq
```
Just to make sure we are working let's test with dig:
``` 
dig this.is.totally.using.local.client.dev @127.0.0.1
```
The response should be very quick and contain something similar to: 
```
;; ANSWER SECTION:
this.is.totally.using.local.client.dev. 0    IN      A       192.168.59.103
```
If you were configuring for pure local not boot2docker the 192.168.59.103 would
be 127.0.0.1.

### Resolve all the things

Now that we have a DNS server running and configured locally, we need to tell OSX how to use dnsmasq. There are a couple of options, but we are going to keep this simple and use resolvers to only affect the client.dev domain.

For more information on what we are about to do see the resolver(5) manual page:
```
man 5 resolver
```

We need a new resolver file, but first make sure you have an /etc/resolver/
directory, if not make one with:
```
mkdir /etc/resolver/
```
Then lets quickly make the file: 
```
sudo tee /etc/resolver/coolwebsite.dev >/dev/null <<EOF
nameserver 127.0.0.1
EOF
```
This file tells OSX to ask our local dns server for any domain in coolwebsite.dev. We should do a quick test make sure all this works:
```
ping -c 1 www.google.com
```
This verifies we didn't break our original DNS. You're good to go if you see something like: 
```
PING www.google.com (74.125.20.105): 56 data bytes
64 bytes from 74.125.20.105: icmp_seq=0 ttl=45 time=34.782 ms
```
Now we should test something new: 
```
ping -c 1 this.will.go.to.my.coolwebsite.dev 
```
Success will look similar to: 
```
PING this.will.go.to.my.coolwebsite.dev (192.168.59.103): 56 data bytes
64 bytes from 192.168.59.103: icmp_seq=0 ttl=64 time=2.973 ms
```
If you configured that for local host you should see 127.0.0.1 instead of the
192.

Finally if you are super lazy and trust me 100 % you can use a bash script to automate this similar to the one you see [here](https://gist.github.com/moofish32/1594c43bdbde2e714ff9#file-dnsmasq_docker-sh)
