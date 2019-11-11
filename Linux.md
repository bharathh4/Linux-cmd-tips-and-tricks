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


# How to solve “device or resource busy” issue in Linux

  - Sometimes if a file is open, you get this issue. Ask all users to close the files or programs. 
  - Or maybe the tool you want is lsof, which stands for list open files. It has a lot of options, so check the man page, but if you want to see all open files under a directory:
  - lsof +D /path
  - That will recurse through the filesystem under /path, so beware doing it on large directory trees. Once you know which processes have files open, you can exit those apps, or kill them with the kill(1) command.
  
# How to get back overwritten or deleted files on Linux

grep -i -a -B100 -A100 'a string unique to your file' /dev/sda1 |
 strings > /var/tmp/my-recovered-file
 
# How to pipe multi line python code to a file
python -c "import dbhelper;dbhelper.summary()" > dbnotes.txt

# How to split text in linux using a delimitter
This shows how to split by the delimitter '#' and select the second element
grep 'None' /tmp/temp.txt | cut -d'#' -f2 - 
