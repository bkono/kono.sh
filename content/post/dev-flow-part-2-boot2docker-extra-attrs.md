+++
Categories = ["development", "docker"]
Description = ""
Tags = ["development", "docker", "boot2docker"]
date = "2015-06-14T21:38:35-07:00"
title = "boot2docker extra attrs"
draft = true

+++

basics of boot2docker
- vm
- linux
- pipes over socket to make things look seamless to osx

what are we setting up with the extra args
- dns by default to consul with fall back (2 worlds of dns)
- makes service.consul service discovery possible from all containers
- resolvers force from host to vm
- vm host args push back to consul dns

### Magic incatantions
sudo ssh magic with b2ddns!

gotchas
- vpn
- boot2docker holding sockets, needs restart
