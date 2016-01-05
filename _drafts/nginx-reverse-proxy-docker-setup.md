---
title: "Dockerized Nginxi RP with Letsencrypt HTTPS"
layout: post
comments: true
---


Introduction
-------

Reverse Proxy (RP) is a type of proxy server that represents other services behind the scene and sends back resources to client as though they were originated from the proxy itself. Jason Wilder blogged [why automated reverse proxy is beneficial in dockerized environment](http://jasonwilder.com/blog/2014/03/25/automated-nginx-reverse-proxy-for-docker/) as well as created a [nginx reverse proxy docker image](https://github.com/jwilder/nginx-proxy). This docker image is able to, on the fly by nginx service reloading, add other popping up web server containers into its cover or remove them when they go offline.

[Let's Encrypt](https://letsencrypt.org/) is a free, automated, and open certificate authority. It offers free-of-charge TLS/SSL certificates for websites to enable encrypted HTTPS traffic.

In this article, we will go through:

1. how to setup a RP service by Jason's docker image 
2. how to download letsencrypt certificates for my existing websites, then enable HTTPS for the RP service

Prerequisites
-------

Before moving forward, I assume you have:

1. An Ubuntu14.04 (or above) server with public IP address in place. Other linux flavors, such as CentOS, should go just fine as well
2. Docker application installed. If not, follow [Install Docker](https://docs.docker.com/engine/installation/) 





Pull and Run Dockerized Nginx Reverse Proxy 
------

Jason has made fetching and using this docker image pretty simple. The command below will pull the docker image if not exists yet locally and run it:

```bash 
$ docker run -p 80:80 -p 443:443 -v ssl-tsl-certificates-path:/etc/nginx/certs -v /var/run/docker.sock:/tmp/docker.sock:ro --rm --name proxy jwilder/nginx-proxy
```

<!-- ```bash  -->
<!-- $ docker run -p 80:80 -p 443:443 \ -->
<!-- -v ssl-tsl-certificates-path:/etc/nginx/certs \ -->
<!-- -v /var/run/docker.sock:/tmp/docker.sock:ro \ -->
<!-- --rm --name proxy jwilder/nginx-proxy -->
<!-- ``` -->

- You should replace `ssl-tsl-certificates-path` with the location where letsencrypt SSL/TSL certificates were previously stored.
- port 443 is open along with port 80 in host machine for the purpose of HTTPS communication
- `--rm` is used so that when the docker container stops the container itself is purged automatically. The purpose is to avoid unused long list of containers sucking up disk spaces
- Do not understand this command? `docker run` [help page](https://docs.docker.com/engine/reference/run/) is your friend

To make sure our container is up and running, `docker ps` should output:

```
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                      NAMES
800beee14cb5        jwilder/nginx-proxy   "/app/docker-entrypoi"   16 hours ago        Up 16 hours         0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   proxy
```

Conclusion
-------

That's it! The nginx RP docker container is up and listening to port 80 and 443 with a free Letsencrypt TLS/SSL certificate to securely serve HTTPS contents. Now we are ready to add dockerized web containers under its umbrella.












<!-- 1. letsencrypt get keys -->
<!-- - git clone, cd letsencrypt, -->
<!-- - ensure port 80 is not used -> ./letsencrypt-auto certonly --standalone -d blog.xixiao.info -->
<!-- 2. copy them to a folder and change to ".crt" and ".key" -->
<!-- 3. ensure to update your keys each 3 months -->
<!-- 4. check in browser the key expiring date -->
<!-- 5. run daemon -->
<!-- 6. `dps` check docker container status -->
