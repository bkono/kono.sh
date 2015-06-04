+++
Categories = ["development", "docker"]
Description = ""
Tags = ["development", "docker", "dnsmasq"]
date = "2015-06-02T21:38:15-07:00"
title = "prodlike-dev-flow part 1 - dnsmasq"

+++

### Why and whats it buy you
dnsmasq is one part of the equation to making our development enviroments behave like a production environment.  Instead of coding in URLs like http://localhost:8080/ we want to provide http://testing.coolwebsite.dev/.  This way if we have websites hosted on subdomains or path extension mounts we can browse and function locally the same as you will with a production environment.  This post solves the DNS problem associated with this flow.  In order to completely get the dev-prod parity additional tools will be necessary, such as load balancers.

### The Setup
```
$> brew install dnsmasq
$> cp /usr/local/opt/dnsmasq/dnsmasq.conf.example /usr/local/etc/dnsmasq.conf
$> sudo cp -fv /usr/local/opt/dnsmasq/*.plist /Library/LaunchDaemons
$> sudo chown root /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
$> sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
```
At this point you now a have locally running dns server.  Let's go configure it to so that we can browse like we are in a production environment.

We are going to take this one step further and prepare ourselves for a future post where we work with 
** need to do this multiline
sed -i.bak 's$#address=\/cowgill\.net\/127\.0\.0\.1$address=/cowgill2.net/127.0.0.1$g' /tmp/dnsmasq.conf
add to the /usr/local/etc/dnsamsq.conf — 2 entries:
address=/docker.dev/127.0.0.1
address=/bspot.dev/127.0.0.1
- configs for bspot.dev

### Resolve all the things
- resolvers

### Dig It
how to test with dig

