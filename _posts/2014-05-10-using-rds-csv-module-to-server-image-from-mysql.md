---
layout: post
category : project
tagline: "using rds_csv module to server image from mysql"
tags : [rds_csv, mysql]
---
{% include JB/setup %}

# background
> i want to server image data from mysql blob field.
> so i can only using "lua_resty_mysql" or "lua_rds_parser" + "ngx_drizzle".
> both of the solution need write LUA code.
> but i want a simple way, just like using "ngx_drizzle" + "rds_json" to build RESTful API, (there's only little command no LUA code).

# solution
> i found "rds_csv", we can config this module just display the blob field.
> but this module escaped this field.
> so i want to add one command to control it, do not escaped this field.
> i have write simple code on the fork https://github.com/lloydzhou/rds-csv-nginx-module/commit/5c2649352fc20d24b26604701118ec7673408877.

    location ~ '/cats/image/([0-9]+)' {
        drizzle_query 'select img from cat_image where id=$1 limit 1';
        drizzle_pass backend;
        rds_csv on;
        rds_csv_quote_string off;
        rds_csv_field_name_header off;
        rds_csv_content_type 'image/jpeg';
    }

** do not display the field name
    rds_csv_field_name_header off;
    
** do not escape the field
    rds_csv_quote_string off;
    
** change the content type
    rds_csv_content_type 'image/jpeg';
