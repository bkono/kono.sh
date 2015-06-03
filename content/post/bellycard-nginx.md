+++
Categories = ["development", "docker", "consul"]
Description = ""
Tags = ["development", "docker", "nginx", "consul", "consul-template"]
date = "2015-06-02T21:38:51-07:00"
menu = "main"
title = "bellycard nginx"

+++

### goal
dynamic loadbalancing
pair with consul for auto-register/deregister via consul-template
make it work like prod using dns in browser -- see dnsmasq setup step

### consul template
go templating language
  handlebar-esque
listens to consul events directly
leads to fast triggered hups of nginx
no chef-client interval to wait for

### the setup
docker pull

exception case of two procs one container
