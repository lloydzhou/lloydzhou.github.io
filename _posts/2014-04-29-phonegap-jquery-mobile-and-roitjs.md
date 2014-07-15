---
layout: post
category : project
tagline: "using phonegap + jqierymobile + roitjs to build android app."
tags : [phonegap, jquerymobile, roitjs, android]
---
{% include JB/setup %}

# write one app on android by using html and javascript.
> jquerymobile can create the ui, and some animation.
> the roitjs can build app into MVP.

# extend roitjs
> roitjs just give us 3 API, $.route(), $.render(), $.observable()
> i extend this framework, add one API: $.model()
> it can create model, and extends by base model.

		var models = {}, guid = function(){return Math.random().toString(36).substring(2, 15)}
		$.model = function(attr, parent) {
		    // create new object: obj = $.model(name, data)
		    // define one class: class = $.model(attr, parent)
		    if (typeof attr == 'string') 
		        return models[attr] ? new models[attr](parent) : {}
		    var pobj = typeof parent == "function" ? new parent() : 
		        typeof parent == 'string' && $.model(parent)
		    return models[attr.name || guid()] = function(data) {
		        $.extend(this, pobj, attr, data)
		        if (!(this.on && this.off && this.trigger && this.one)) $.observable(this)
		        this.name = attr.name || this.name || arguments.callee.name || 'base'
		        this[this.name+'Init'] && this[this.name+'Init'](this)
		    }
		} 

> the attr is the attribute or method defined on "Model".
> the parent is the base model. can be null.
> the data is the current attributes to create new object by using "new". 
