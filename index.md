---
layout: page
title: 木木的主页
header: Posts
---
{% include JB/setup %}

# Overview  

This is my own personal page on the github.com. 

## Projects 

### [luajit-mongo](https://github.com/lloydzhou/luajit-mongo.git)  

> wraper mongo driver by using ffi.
> using one bson library [lua-cbson](https://github.com/lloydzhou/lua-cbson.git), based on [libbson](https://github.com/mongodb/libbson.git)

### [ease](https://github.com/lloydzhou/ease.git)  

> A easeing function component.
> Include 30 ease functions.
> can see the description on site: [easings.net](http://easings.net/) or see it in the test file.

### [mapped.io](https://github.com/lloydzhou/mapred.io.git )  

> A mapreduce framework based on socket.io write in node's. 
> Using a nodejs server to control the maper and reducer. 
> Using the browser to run tasks 
> Every node communication with the controller by using socket.io, (Html5 browser just using Web Socket). 
> Update the mapred.io-client.js file, can using in browser and server, so we can add node as a standalone application in server not just using browser.  

### [microtpl](https://lloydzhou.github.io/microtpl/)  

> Small template attribute language implemention for PHP (using xml_parse)

### [ActiveRecord](http://lloydzhou.github.io/activerecord/)  

> Simple activerecord in PHP.  
> Easy to using(can using chain calls).  
> Support three relations (HAS_ONE, HAS_MANY, BELONGS_TO).   
> Only one file(398 lines with comments).   

### [editjson](https://lloydzhou.github.io/editjson/)  

> A jQuery plugin to edit json data.
> using "ul" "li" to view the json data as a tree.
> there's 3 buttons to "add after", "insert" and "delete" the node.
> register "dbclick" event to edit the key and value (if the content is so long, using textarea instead of input to edit it). 

### [jtable](https://github.com/lloydzhou/jtable.git)   

> forked this project from other git user, using restful request. 
> add one plugin to filter records. just extended from the creation form and editing form. 

### [YII extension](https://github.com/lloydzhou/yii.extension.git)   

> using YII framework to wrap the jtable, can using this extension to edit the Active Record on admin panel. can edit the relation AR (HAS_MANY) by open a children table. 

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


