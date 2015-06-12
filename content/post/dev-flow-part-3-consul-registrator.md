+++
Categories = ["development", "docker", "consul"]
Description = ""
Tags = ["development", "docker", "consul", "registrator"]
date = "2015-06-02T21:38:43-07:00"
title = "consul registrator"

+++

consuls role in local dev
- service discovery
    - things like redis/postgres/rabbit all go only internal to the docker containers, accessed via
        service discovery dns
- dns
- tie in to nginx for local routing

why registrator
- listens to docker events
- easy service naming & tagging
- get fancy-like with automatic scoping (another post around this idea for docker & consul in prod)

### The setup
docker pull
--restart=always
port mappings ... lots for consul. short why
registrator, nothing exposed, and why

ui screen shots

lead in to boot2docker extra args ... part 3
