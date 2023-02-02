haproxy-keepalived
====================

Demo of HAProxy high availability using Docker.

This project sets up the following docker containers on a private network:

* haproxy1 (192.168.3.9)
* haproxy2 (192.168.3.10)
* web1 (192.168.3.11)
* web2 (192.168.3.12)
* client (192.168.3.16)

The HAProxy servers are themselves made redundant via keepalived, with
virtual IP 192.168.3.2.

# Prerequisites

1.  Install [Docker](https://docs.docker.com/engine/install/)
2.  Install [Docker compose](https://docs.docker.com/compose/install/)
3.  Either clone this repo, or download the zip file and extract it to an empty directory.

# Getting started

* From the root directory, 
    run `docker compose build` to build the scenario
    run `docker compose up -d` to start the containers
* View the various pages:
  * source web sites: e.g., http://192.168.3.11
  * web site, load-balanced via HA Proxy: e.g., http://192.168.3.9
  * web site, through the virtual IP: http://192.168.3.2
  * HA proxy status: e.g., http://192.168.3.2/haproxy?stats

## Determining the active HAProxy

keepalived is an implementation of the VRRP (Virtual Router Redundancy
Protocol) protocol to make IPs highly available - a so called VIP
(Virtual IP).

You can check which HAProxy server is currently handling requests to the virtual IP 192.168.2.2 by checking the server's ip addresses:

    $ docker compose exec haproxy1  /bin/sh -c "echo haproxy1; ip addr | grep 192.168.3.2/32"
    haproxy1
        inet 192.168.3.2/32 scope global eth0
    Connection to 127.0.0.1 closed.
    $ docker compose exec haproxy2  /bin/sh -c "echo haproxy1; ip addr | grep 192.168.3.2/32"
    haproxy2
    Connection to 127.0.0.1 closed.

## Scenarios to investigate

Try turning various servers on and off, and visit any of the URLs to
see what the results are.  As HAProxy services are turned on and off,
also check the active HAProxy.

Examples:

* `docker compose container_name stop`
* `docker compose container_name start`


# Shutting down

* `docker compose stop` turns off service containers
* `docker compose rm` removes stopped service containers
  cached images
* `docker system prune` removes the containers.