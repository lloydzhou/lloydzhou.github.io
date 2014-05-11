---
layout: post
category : project
tagline: "using imagemagic to create thumbnails"
tags : [imagemagic, lua, mysql]
---
{% include JB/setup %}

# background
> when build web applications, we always want to create many thumnails. not using the origin images.

# solution
> using the imagemagic's "convert" command to create thumbnail

  convert imput -thumbnail 80x80 -gravity center -extent 80x80 output
  
> we can also using this command to get input image from stdin, and output thumbnail to stdout.
> so we can using LUA function "io.popen" to execute the convert command, and output the thumbnail to the response.

    local fp = assert(io.popen("convert " .. ngx.var.image ..
      " -thumbnail 80x80 -gravity center -extent 80x80 - 2>/dev/null"), 
      "convert error, can not open pipe.")
    local content = fp:read("*all")
    fp:close()
    ngx.print(content)

> my image data was stored into mysql, but the function "io.popen" can not open one pipe, to read and write data.
> so we need another method to get the image data from mysql.
> mysql -e option can exec the SQL command.
> mysql --raw option, just write the resultset to output.
> mysql --skip-column-names option, do not display the column names.
> so using this options, can just write the blob field to stdout.
> and then pipe it to convert command.

    local fp = assert(io.popen("mysql -h$OPENSHIFT_MYSQL_DB_HOST -P$OPENSHIFT_MYSQL_DB_PORT "..
      "-u$OPENSHIFT_MYSQL_DB_USERNAME -p$OPENSHIFT_MYSQL_DB_PASSWORD -D api "..
       "--disable-pager -e \'".. ngx.var.sql .."\' --raw --skip-column-names | convert - " ..
       " -thumbnail 80x80 -gravity center -extent 80x80 - 2>/dev/null"), 
       "convert error, can not open pipe.")
    local content = fp:read("*all")
    fp:close()
    ngx.print(content)
                
> perfomance of this solution was not good, so we need cache for this request, "https://github.com/openresty/srcache-nginx-module" is a good choice.
