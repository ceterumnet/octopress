---
layout: post
title: "using git with a proxy"
date: 2010-05-03 14:09
comments: true
categories: code
---
Some of us are stuck behind a corporate firewall, but need to access all the great little github plugins through git!  So what do you do?

Create the following wrapper:


``` bash ( ~/bin/proxy-wrapper ):

!/bin/sh

Put your own values

PROXY_IP=127.0.0.1
PROXY_PORT=1090

nc -x${PROXY_IP}:${PROXY_PORT} -X5 $*
```

``` bash add this to your ~/.profile or ~/.bash_rc etc…
export GIT_PROXY_COMMAND=~/bin/proxy-wrapper
```

I stole this solution from <a href="http://blogs.gnome.org/juanje/2009/07/17/git_behind_proxy/">http://blogs.gnome.org/juanje/2009/07/17/git_behind_proxy/</a>



Cheers, Dave