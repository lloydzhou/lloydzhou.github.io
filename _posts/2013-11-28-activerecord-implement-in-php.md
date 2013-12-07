---
layout: post
category : project
tagline: "simple activerecord implement in PHP"
tags : [php, activerecord, tinymvc]
---
{% include JB/setup %}

# ActiveRecord
> Simple implement of active record in PHP.  
> Using magic function to implement more smarty functions.  
> Can using chain method calls, to build concise and compactness program.  

> Easy to using(can using chain calls).  
> Support three relations (HAS_ONE, HAS_MANY, BELONGS_TO).   
> Only one file(398 lines with comments).   

## WHERE condition
> maping the function name and the operator, to build Expressions in WHERE condition.  

	user can call it like this: 
		 $user->isnotnull()->eq('id', 1); 
	will create Expressions can explain to SQL: 
		 WHERE user.id IS NOT NULL AND user.id = :ph1

## sqlParts 
> maping the function name and the operator to build SQL Part.  

	call function like this: 
		 $user->order('id desc', 'name asc')->limit(2,1);
	can explain to SQL:
		 ORDER BY id desc, name asc limit 2,1

## [Class Reference](http://lloydzhou.github.io/activerecord/)


