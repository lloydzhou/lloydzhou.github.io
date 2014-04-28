---
layout: post
category : project
tagline: "using srcache module to cache page"
tags : [nginx, module, openshift, cache]
---
{% include JB/setup %}

# background
> i have make one app on openshift. this is the backend restful server. 
> Get data from mysql and output into json and image format.
> using ngx_drizzle to talk to mysql server, this api is so slow.
> so i want to using srcache module to cache the page.

# issue
> when i first using this module this module hit the cache from redis, but always send 200 status, and send the response body.
> so i want to validate the etag and last modified header(https://github.com/openresty/srcache-nginx-module/issues/29). if validated then send 304 status code.
> it can make this api faster. so the client app will run faster.

