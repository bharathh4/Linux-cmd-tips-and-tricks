###########
Setup mosh
###########

On the server side

::

    su
    yum install -y epel-release
    yum install -y mosh
    firewall-cmd --zone=public --permanent --add-port=60000-61000/udp
    yum install firewall-cmd
    sudo iptables -I INPUT 1 -p udp --dport 60000:61000 -j ACCEPT
    sudo service iptables save
    

    pkill mosh-server
    mosh-server

      MOSH CONNECT 60001 PtPrjgP/VIO5zVfu8hwI4Q
      
On the client side

If using mobaxterm 
::

    Make sure that
    - ssh port = 22
    - mosh udp port = 60001 (from the mosh-server)
    - charset = auto
    - prediction type = adaptive
    - mosh-server path = mosh-server

If using chrome mosh client

  Work in progress
    
