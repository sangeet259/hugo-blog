+++
draft = false
date = 2019-02-18
title = "Set Proxy for all the Docker files"
slug = "proxy-for-docker-files"
tags = ["docker","devops"]
categories = [ "TIL"]
+++


So I was using a docker container.
Docker was already infmaous in my eyes because the way it sets up proxy. I remember wasting a lot of time initially only to find out that you have to create som `proxy-conf.d` file in order to enable proxy usange in docker.


Now came using Docker Images. So I had this image where I had a pip install running, when I `docker-compose up`-ed, there seemed some glitch, but it was familiar, it striked me, it was PROXY issue !!

So I searched on how to enable proxy inside docker containers, so as to enable them use the network.
Some answers I got were telling me to use `ENV` command and then pass a normal bash line setting the `http_proxy` and others. But an immediate drawback struck me, it is using the proxy of it's host system, and it's hard coded, now if share this container with someone she has to check the proxy, change/remove it. Damn ! Too much work.

Fortunately the Docker team has introduce setting proxy in `~/.docker/cofig.json`.
All I had to do was, place this in that file:
```json
{
 "proxies":
 {
   "default":
   {
     "httpProxy": "http://172.16.2.30:8080",
     "httpsProxy": "http://172.16.2.30:8080",
     "ftpProxy": "http://172.16.2.30:8080",
     "noProxy": "10.*.*.*, localhost, 127.0.0.1"
   }
 }
}
```

And it worked like charm, the pip and apt command in other images were working. 
You don't have to `ENV https_proxy 'http://ip:3128'
` in all the Docker Files!

Hurray !! :tada: