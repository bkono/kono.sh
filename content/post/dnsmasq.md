+++
Description = ""
tags = ["development", "devops", "docker", "dnsmasq"]
date = "2015-06-02T21:38:15-07:00"
title = "A Prodlike dev flow part 1 - dnsmasq "
author = "moofish32"

+++

### DNS for development just like your production setup

dnsmasq is one part of the equation to making our development environments behave like a production
environment.  Instead of coding in URLs like http://localhost:8080/ we want to provide
http://testing.coolwebsite.dev/.  This way we can have websites hosted on subdomains or we browse
and function locally the same as you will with a production environment.  In addition, we are planning to use [Consul](https://www.consul.io) for service discovery which leverages DNS. This post solves the DNS problem associated with this flow.  In order to completely get the [dev-prod](http://12factor.net/dev-prod-parity) parity additional tools will be necessary, such as load balancers or reverse proxies (think nginx and a follow up post to elaborate).

### The Setup
```
$> brew install dnsmasq
$> cp /usr/local/opt/dnsmasq/dnsmasq.conf.example /usr/local/etc/dnsmasq.conf
$> sudo cp -fv /usr/local/opt/dnsmasq/*.plist /Library/LaunchDaemons
$> sudo chown root /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
$> sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
```
At this point you now a have locally running dns server.  Let's go configure it so that we can browse like we are in a production environment.

Now we need to tell our DNS server where to resolve our development environmnet.  I typically use the domain name with a dev top level domain.  For instance if I was working on http://www.cowgill.com my test domain is *.cowgill.dev.  This is configured by adding the name address pair as follows: 
```
sed -i.bak '/#address=\/double-click\.net\/127\.0\.0\.1/a\
  address=/cowgill.dev/192.168.59.103\' /usr/local/etc/dnsmasq.conf
```
The whitespace in this command for OSX must match exactly or you will discover the GNU vs BSD sed differences. As an alternative you can just open /usr/local/etc/dnsmasq.conf in your favorite editor and append this line.

This setup is preparing us to pair with [Docker Compose](https://docs.docker.com/compose/). If you are not going to use Docker Compose, then you probably just want to use the loopback address as below:
```
sed -i.bak '/#address=\/double-click\.net\/127\.0\.0\.1/a\
  address=/cowgill.dev/127.0.0.1\' /usr/local/etc/dnsmasq.conf
```

### Resolve all the things
- resolvers

### Dig It
how to test with dig

