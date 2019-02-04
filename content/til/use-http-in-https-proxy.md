+++
draft = false
date = 2019-01-21
title = "HTTP in HTTPS proxy"
slug = "http-in-https"
tags = ["devops","proxy","docker"]
categories = [ "TIL"]
+++

Whenever I set an `https_proxy`, I am tempted to do something like :

`https_proxy=https://172.16.2.30:8080/`

Notice the **"https://"** , sadly that's not how it works, you have to set it **"http://"** only.

I have previously been caught in problems due to this. The latest one was when I used the **https://** in the docker proxy configuration.

Thankfully this stackoverflow answer saved the day : https://stackoverflow.com/a/51648635/6507303