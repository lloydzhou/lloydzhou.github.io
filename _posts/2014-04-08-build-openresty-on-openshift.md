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
    
    wget http://openresty.org/download/drizzle7-2011.07.21.tar.gz
    tar -zxvf drizzle7-2011.07.21.tar.gz

    export PATH=/sbin:$PATH
    export LD_LIBRARY_PATH=$OPENSHIFT_DATA_DIR/usr/local/lib:/opt/rh/nodejs010/root/usr/lib64:$LD_LIBRARY_PATH
    export PKG_CONFIG_PATH=$OPENSHIFT_DATA_DIR/usr/local/lib/pkgconfig

    
    cd drizzle7-2011.07.21
    ./configure --without-server --prefix=$OPENSHIFT_DATA_DIR
    make libdrizzle-1.0
    make install-libdrizzle-1.0
    

    cd ngx_openresty-1.5.11.1
    ./configure --prefix=$OPENSHIFT_DATA_DIR --with-pcre=$OPENSHIFT_TMP_DIR/pcre-8.33 \
    --with-luajit --with-libdrizzle=$OPENSHIFT_DATA_DIR --with-http_drizzle_module
    
    gmake && gmake install
    
    
