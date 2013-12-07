---
layout: post
category : project
tagline: "Pull request to dispatch"
tags : [php, dispatch, tinymvc]
---
{% include JB/setup %}

#[contributors](https://github.com/noodlehaus/dispatch#credits-and-contributors)

## dispatch
> dispatch is a tiny mvc framework in php.  
> it give us 25 functions, help us easily to build mvc web application.  

## Add helper function 
> when i using it to build one [demo](https://github.com/lloydzhou/tinymvc) web application.  
> i want to get the pathinfo.  
> but i found the same code in old function "dispatch", so i move this code outside that function.  
> and add one helper function named "path". pull this request into the parent code tree.  

## Merge request_body into params
> the old version have function named "params" to get params from "GET" and "POST".  
> but we always want it to get the request_body when using other request type.  
> so i merge the request_body into function "params".  

* noodlehaus have merge the two point into code tree [pull #24](https://github.com/noodlehaus/dispatch/pull/24)

## found issue 
> when i merge the "request_body" into "params".  
> i found that this function read data from stream "php://input".  
> this stream can read once.  
> so if user call this function next time, can get nothing.  

* noodlehaus have fixed it [issue #25](https://github.com/noodlehaus/dispatch/issues/25)
