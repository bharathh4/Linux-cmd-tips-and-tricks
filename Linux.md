# How to grep an entire directory

- grep -s "search_token" * .*
- grep -s "search_token" /path/to/dir/*
- grep -rl "search_token" /path

  
# How to open a port in firewall 

  > ## Ubuntu

    >> ### Say you want to open port 5002
    sudo ufw allow 5002
  > ## CentOS

    su
    iptables -I INPUT -p tcp --dport 5000 --syn -j ACCEPT (incoming)
    iptables -I OUTPUT -p tcp --dport 5000 --syn -j ACCEPT (outgoing) optional
    service iptables save
    Ctrl-D
    
 # How to quickly grep for a string in a sqlite file
 
 sqlite3 your.db .dump | grep Nuance

# How to add an alias for quick access permanently
- alias gm='cd /home/speech/Scripts/somedir/'
- alias speechops='source /home/speech/Scripts/speech_ops_venv/bin/activate'
Do 
 - vim ~/.bash_aliases
 - Insert above alias defs


