---
layout: page
title: 木木的主页
header: Posts
---
{% include JB/setup %}

# Overview  

This is my own personal page on the github.com. 

## Projects 
* [mapped.io](https://github.com/lloydzhou/mapred.io.git )
> A mapreduce framework based on socket.io write in node's. 
> using a nodejs server to control the maper and reducer. 
> using the browser to run tasks 
> every node communication with the controller by using socket.io, (Html5 browser just using Web Socket. 
* [editjson](https://github.com/lloydzhou/editjson.git ) 

## Posts list

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


## Help
Read [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)

Complete usage and documentation available at: [Jekyll Bootstrap](http://jekyllbootstrap.com)

This theme is still unfinished. If you'd like to be added as a contributor, [please fork](http://github.com/plusjade/jekyll-bootstrap)!
We need to clean up the themes, make theme usage guides with theme-specific markup examples.


