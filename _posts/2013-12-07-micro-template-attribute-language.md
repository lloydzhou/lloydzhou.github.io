---
layout: post
category : project
tagline: "small template attribute language implement for PHP (using xml_parse) "
tags : [php, tal, template, template attribute language, xml parse, tinymvc]
---
{% include JB/setup %}

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
