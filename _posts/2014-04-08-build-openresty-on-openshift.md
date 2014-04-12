---
layout: post
category : project
tagline: "build openresty on openshift"
tags : [openresty, openshift]
---
{% include JB/setup %}

# create openshift app
> 

# build openresty


    cd $OPENSHIFT_TMP_DIR
    wget http://openresty.org/download/ngx_openresty-1.5.11.1.tar.gz
    wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.33.tar.gz
    tar -zxvf pcre-8.33.tar.gz 
    tar -zxvf ngx_openresty-1.5.11.1.tar.gz 
    
    export PATH=/sbin:$PATH
    export LD_LIBRARY_PATH=$OPENSHIFT_DATA_DIR/usr/local/lib:/opt/rh/nodejs010/root/usr/lib64:$LD_LIBRARY_PATH
    export PKG_CONFIG_PATH=$OPENSHIFT_DATA_DIR/usr/local/lib/pkgconfig
    
    ./configure --prefix=$OPENSHIFT_DATA_DIR --with-pcre=$OPENSHIFT_TMP_DIR/pcre-8.33 --with-luajit 
    
    make && make install
    
    
