---
layout: post
category : project
tagline: "using ssh port forearding to access remote mysql server"
tags : [mysql, ssh, openshift]
---
{% include JB/setup %}

# install autossh

    wget http://www.harding.motd.ca/autossh/autossh-1.4c.tgz 
    gunzip -c autossh-1.4c.tgz | tar xvf -
    cd autossh-1.4c
    ./configure
    make
    
    copy binary to where you wish it, or "make install" will install it under /usr/local by default.
    examine autossh.host for example wrapper script and options
    
# create an port forwarding script

    SSH=" -F $OPENSHIFT_DATA_DIR/ssh_config <remote uuid>@<remote app namespace>.rhcloud.com "
    REMOT_MYSQL_HOST=$(ssh $SSH printenv OPENSHIFT_MYSQL_DB_HOST)
    REMOT_MYSQL_PORT=$(ssh $SSH printenv OPENSHIFT_MYSQL_DB_PORT)
    $OPENSHIFT_DATA_DIR/autossh -M0 -f -N -L $OPENSHIFT_PHP_IP:19999:$REMOT_MYSQL_HOST:$REMOT_MYSQL_PORT $SSH
    
forwarding the remote applications mysql port to local server on port 19999.

