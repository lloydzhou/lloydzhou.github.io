---
layout: post
category : project
tagline: "add the rds_json_ssi command to merge the RDS resultset"
tags : [rds, ssi, mysql]
---
{% include JB/setup %}

# background  
using the ngx_drizzle + rds_json, can easy to create RESTful API server data from MYSQL.
but some times we need merge source in one requests it can not using join to get the data from mysql...

# solution  
so i add this command to using the "server side include" (SSI) to do this feature.
the command "rds_json_ssi" need 3/4 params:
* key ==> the field name
* prefix ==> the SSI url prefix
* property ==> the new field name for the content from SSI url.

#demo
## data
the exampe data from the Yii framework demos.
https://code.google.com/p/yii/source/browse/trunk/demos/blog/protected/data/schema.mysql.sql

## nginx config
> define the upstream

    upstream mysql{
        drizzle_server 127.0.0.1:3306 dbname=blog password=123 user=root protocol=mysql charset=utf8;
    }

###  define the locations
> find post by id, this is a detail API, so need display the author and comments list.   
> using command "rds_json_ssi author_id /user/ author;"   
> it will add attribute "author" valued by "<!--# include virtual="/user/{id}" -->"   

	location ~ '/post/([0-9]+)' {
		ssi on;
		ssi_types application/json;
		drizzle_query 'select * from `tbl_post` where id=$1';
		drizzle_pass mysql;
		rds_json on;
		rds_json_ssi author author_id /user/ ;
		rds_json_ssi comments id /comments/ ;
	}

> find user by id

	location ~ '/user/([0-9]+)' {
		drizzle_query 'select username,email,profile from tbl_user where id=$1';
		drizzle_pass mysql;
		rds_json on;
	}
	
#### output of post detail
> the comments is a list, but one post only belong to one author. so can not only using join to output this resultset.

	[
		{
			id: 2,
			comments: [
				{
					id: 1,
					content: "This is a test comment.",
					status: 2,
					create_time: 1230952187,
					author: "Tester",
					email: "tester@example.com",
					url: null,
					post_id: 2
				}
			],
			title: "A Test Post",
			content: "Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
			tags: "test",
			status: 2,
			create_time: 1230952187,
			update_time: 1230952187,
			author_id: 1,
			author: [
				{
					username: "demo",
					email: "webmaster@example.com",
					profile: null
				}
			]
		}
	]

> find the comments list by post id

	location ~ '/comments/([0-9]+)' {
		drizzle_query 'select * from tbl_comment where post_id=$1';
		drizzle_pass mysql;
		rds_json on;
	}

> find the post list by tag name, in this list also need author. but not need comment list..

	location ~ '/tag/([a-z_0-9]+)' {
		ssi on;
		ssi_types application/json;
		drizzle_query 'select * from tbl_post where tags like \'%$1%\'';
		drizzle_pass mysql;
		rds_json on;
		rds_json_ssi author author_id /user/ .json;
	}
