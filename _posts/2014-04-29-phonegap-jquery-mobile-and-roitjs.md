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

	$.model = function(attr, parent) {
		var pobj = typeof parent == "function" && new parent() || {}
		return function(data) {
			$.observable($.extend(this, pobj, attr, data))
		}
	}

> the attr is the attribute or method defined on "Model".
> the parent is the base model. can be null.
> the data is the current attributes to create new object by using "new". 
