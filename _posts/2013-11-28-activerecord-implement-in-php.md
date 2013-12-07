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












# [TAL](http://en.wikipedia.org/wiki/Template_Attribute_Language)
> template attribute language discription from "wikipedia".  
> The Template Attribute Language (TAL) is a templating language used to generate dynamic HTML and XML pages.  
> Its main goal is to simplify the collaboration between programmers and designers.  
> This is achieved by embedding TAL statements inside valid HTML (or XML) tags which can then be worked on using common design tools.  
> TAL was created for Zope but is used in other Python-based projects as well.  

## My implement in php [Microtpl](http://lloydzhou.github.io/microtpl/)
> MicroTpl is small templating system for PHP.
> implement some syntax of template attribute language in PHP (using xml_parse)

### Syntax
	tal:content="$title"
	tal:condition="isset($messages)"    
	tal:repeat="$messages as $key => $message"
	tal:replace=""
	/**
	 * Replace the attributes of the tag with php code. 
	 * Can using the attribute names such as 'id', 'href', 'class' and so on.
	 */
	tal:id="'message-'.($key+1)"
	tal:href="'#message-'.($key+1)"
	tal:class="($key%2 ? 'odd' : 'even')" 
	......

### API refrence

	// parse the template from string 
	public static function parse($template)
	//  render file with layout by using $data.
	public static function render($view, $data = array(), $layout = '') 
