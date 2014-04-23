---
layout: post
category : project
tagline: "add memcached and redis to openshift"
tags : [memcached, redis, openshift]
---
{% include JB/setup %}

## the cartridge install url

```
  smarterclayton-redis-2.6 (Redis)
  --------------------------------
    From:    http://cartreflect-claytondev.rhcloud.com/reflect?github=smarterclayton/openshift-redis-cart
    Website: https://github.com/smarterclayton/openshift-redis-cart

  brianredbeard-memcached-1.4.15 (Memcached Distributed Memory Object Caching System)
  -----------------------------------------------------------------------------------
    From:    http://reflector-getupcloud.getup.io/reflect?github=getupcloud/openshift-origin-cartridge-memcached
    Website: http://www.memcached.org
```

> by the way, i have add a nginx server on openshift, so i config the php-cgi backend the nginx.
> start php-cgi
```
    if [ -z "$(ps -ef | grep php-cgi | grep -v grep)" ]
    then
        client_result "PHP has stopped, start it."
        nohup /usr/bin/php-cgi -b $OPENSHIFT_DIY_IP:9000 > /tmp/server.log 2>&1 & 
    else
        client_result "PHP is running, stopped it first."
        pkill php-cgi
        nohup /usr/bin/php-cgi -b $OPENSHIFT_DIY_IP:9000 > /tmp/server.log 2>&1 & 
    fi
```
> stop php-cgi
```
    if [ -z "$(ps -ef | grep php-cgi | grep -v grep)" ]
    then
        client_result "PHP is already stopped"
    else
        pkill php-cgi
    fi
```