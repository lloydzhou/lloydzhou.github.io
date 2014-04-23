---
layout: post
category : project
tagline: "using ngx_drizzle to make RESTful service"
tags : [openresty, mysql, ngx_drizzle, openshift]
---
{% include JB/setup %}

# install ngx_drizzle
> should be install lib_drizzle first.
> using openresty, it's include many module.

# codding
> just make a nginx config file.
> in this config file, will using some env var. so using sed to replace it before deploy the app.



> build every api by define location, output to JSON format.
```
    location ~ '/api/lookup/([a-z0-9_]+)' {
        set_quote_sql_str $type $1;
        drizzle_query 'select * from `lookup` where `type`=$type';

        # the belong is always same, can move it into another template file, and include it.
        add_header Access-Control-Allow-Origin '*';
        add_header Access-Control-Allow-Methods 'PUT, POST, GET, OPTIONS';
        add_header Access-Control-Allow-Headers 'X-Requested-With';
        add_header Access-Control-Allow-Credentials 'true';
        drizzle_pass backend;
        rds_json on;
    }
```

> add the "Access-Control-Allow-*" header to support crosse domain ajax call.
> need XMLHttpRequest level 2, in IE 8 there's XDomainRequest object to support.