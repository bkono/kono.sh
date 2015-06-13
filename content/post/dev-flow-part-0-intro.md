+++
tags = ["development", "devops", "docker", "dnsmasq"]
comments = false
date = "2015-06-02T22:33:47-07:00"
description = ""
image = ""
menu = ""
share = true
title = "A prodlike dev flow part 0 - intro"

+++

Bringing docker into the picture for deployments to production has a number of great benefits. A key
one of those is the fact that we can work with the same container locally as we will during a
deployment to prod. In a distributed system composed of multiple services, this benefit grows exponentially if
we invest a little time up front to integrate system testing on our local development environments.
Being able to snag down all of the current containers deployed to prod is a great first step, but
you'll quickly find a few gaps remain. In this series of articles on a prod-like development flow,
we'll attempt to tackle all of the major points with a description of the tool, why you need it, and
how to get it all setup. If all goes according to plan (always does, right?) then we'll end up with
a simple set of commands to spin up an almost replica of production, execute system-wide integration
specs, and even browse to it on a local subdomain for the always needed but much maligned manual
testing.

As posts are completed, I'll keep the TOC below updated. Keep an eye out for frequent updates over
the next couple weeks.

1. [DNS with dnsmasq](/post/dev-flow-part-1-dnsamsq)
1. Boot2docker Extra Attrs
1. Consul & Registrator
1. Nginx & Consul-template

