---
layout: post
category : project
tagline: "using system env in nginx config"
tags : [openresty, openshift, nginx, env]
---
{% include JB/setup %}

    sed -e "s,`echo '$OPENSHIFT_REPO_DIR'`,`echo $OPENSHIFT_REPO_DIR`," \
        -e "s,`echo '$OPENSHIFT_DIY_IP'`,`echo $OPENSHIFT_DIY_IP`," \
        -e "s,`echo '$OPENSHIFT_DIY_PORT'`,`echo $OPENSHIFT_DIY_PORT`," \
        -e "s,`echo '$OPENSHIFT_MYSQL_DB_PORT'`,`echo $OPENSHIFT_MYSQL_DB_PORT`," \
        -e "s,`echo '$OPENSHIFT_MYSQL_DB_HOST'`,`echo $OPENSHIFT_MYSQL_DB_HOST`," \
        -e "s,`echo '$OPENSHIFT_MYSQL_DB_PASSWORD'`,`echo $OPENSHIFT_MYSQL_DB_PASSWORD`," \
        -e "s,`echo '$OPENSHIFT_MYSQL_DB_USERNAME'`,`echo $OPENSHIFT_MYSQL_DB_USERNAME`," \
        $OPENSHIFT_REPO_DIR/.openshift/action_hooks/nginx.conf.template \
        > $OPENSHIFT_DATA_DIR/nginx/conf/nginx.conf