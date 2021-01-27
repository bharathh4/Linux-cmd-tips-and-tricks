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
    
  > ## CentOS 8
    
    firewall-cmd --list-all
    firewall-cmd --get-services
    firewall-cmd --get-zones
    firewall-cmd --zone=public --permanent --add-service=http
    firewall-cmd --zone=public --permanent --add-port 8080/tcp
    firewall-cmd --reload
    firewall-cmd --list-all
    
# How to close a port in firewall

  > ## CentOS 8
  
    firewall-cmd --zone=public --permanent --remove-service http
    firewall-cmd --zone=public --permanent --remove-port 8080
    firewall-cmd --reload
    
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
- This shows how to split by the delimitter '#' and select the second element
- grep 'None' /tmp/temp.txt | cut -d'#' -f2 - 

# How to do inverse pattern match using grep in linux
grep -v 'None' /tmp/temp.txt

# How to grep for only the pattern in linux 
grep -o "hello.*" 'crazyhello.wav' 

# How to add a harddrive in Linux
- Assume you have added a physical drive or added a drive in vitualbox
- ls /dev/sd*
- p
- n
- Enter
- Enter 
- Enter
- w

#### Creating a Filesystem on an Ubuntu Disk Partition
- sudo mkfs.ext4 -L /hdd2 /dev/sdb1

#### The drive is called hdd2
- sudo mkdir /hdd2
- sudo mount /dev/sdb1 /hdd2
- sudo gedit /etc/fstab
- /dev/sdb1       /hdd2         auto    defaults        0       0


# How to search for keywords in directories in linux
- grep -r -o "your_word" .*
- grep -r -o "your_word" your_directory

# How to extend a drive on Ubuntu on Virtualbox

- On the host machine (Windows)
- Release the VDI file: File -> Virtual Media Manager -> Select VDI -> Release
- Backup the VDI file
- Open a command prompt and browse to:
  ```
  cd C:\Program Files\Oracle\VirtualBox
  ```
- Resize your .vdi file:
- $.\VBoxManage modifyhd 'C:\Users\jochen\VirtualBox VMs\Ubuntu\Ubuntu.vdi' --resize 40000 # 40 GB disk
- Startup your virtual machine
- On the guest machine (Ubuntu)
- Install & start gparted:
  ```
  sudo apt-get install gparted
  gparted
  ```
- Get rid of the swap partition, which prevents you from expanding the root partition. Note that you cannot harm the rest of your machine - this is all happening inside a single file. Worst case scenario you trash this file and you have to use your backup instead.
- Make a note of the size of the linux-swap partition 4 GB in my case
- Right click on it and swapoff
- Right click on it and Delete
- Apply by clicking on the checkmark (Apply all operations). Ignore the warning
- Right click on the extended file system that once housed the swap partition (/dev/sda2 in all likelihood) and delete it
- Right click on the root partition (/dev/sda1) and resize it. Tab to the 'Free space following' field and enter the size of the swap partition. Shift-tab and the machine will work out the new size for you automatically
- Right click in the unallocated space at the end and make it an extended partition
- Right click in the new partition and select linux-swap in the File system field.
- Commit your changes as before
- Right click on your swap partition and select swapon

# How to copy a directory in Linux
- scp -P 2222 -r speech@10.0.0.6:/home/speech/Scripts/S3uploads S3uploads

# What to do when you can't ssh into a machine
- check port rules
  - is port 22 allowed. In Azure, the rules at the bottom most are the most restrictive and the ones you add on top are exeptions and allowances.
- check if the sshd deamon is running; if it isn't debug using systemctl. 
  - Check if directory permissions has changed. For example if /var is not owned by root, sshd cannot start. /var should always be owned by root with individuals having read and write privileges. 
  
- check if you are using the right .pem file. 
  - do you have the right private key.
  
 # How to structure development apps in Linux
 - Source https://unix.stackexchange.com/questions/11544/what-is-the-difference-between-opt-and-usr-local
   - While both are designed to contain files not belonging to the operating system, /opt and /usr/local are not intended to contain the same set of files. /usr/local is a place to install files built by the administrator, typically by using the make command (e.g., ./configure; make; make install). The idea is to avoid clashes with files that are part of the operating system, which would either be overwritten or overwrite the local ones otherwise (e.g., /usr/bin/foo is part of the OS while /usr/local/bin/foo is a local alternative).

   - All files under /usr are shareable between OS instances, although this is rarely done with Linux. This is a part where the FHS is slightly self-contradictory, as /usr is defined to be read-only, but /usr/local/bin needs to be read-write for local installation of software to succeed. The SVR4 file system standard, which was the FHS' main source of inspiration, is recommending to avoid /usr/local and use /opt/local instead to overcome this issue.

   - /usr/local is a legacy from the original BSD. At that time, the source code of /usr/bin OS commands were in /usr/src/bin and /usr/src/usr.bin, while the source of locally developed commands was in /usr/local/src, and their binaries in /usr/local/bin. There was no notion of packaging (outside tarballs).

   - On the other hand, /opt is a directory for installing unbundled packages (i.e. packages not part of the Operating System distribution, but provided by an independent source), each one in its own subdirectory. They are already built whole packages provided by an independent third party software distributor. Unlike /usr/local stuff, these packages follow the directory conventions (or at least they should). For example, someapp would be installed in /opt/someapp, with one of its command being /opt/someapp/bin/foo, its configuration file would be in /etc/opt/someapp/foo.conf, and its log files in /var/opt/someapp/logs/foo.access.

- https://superuser.com/questions/728659/where-should-i-install-my-app-on-linux
   - This is a question with no right answer and an intriguing bit of Unix history.
   - The rule we followed at my prior employer was that non-out-of-the-box software was installed in /opt/PackageName-VersionNumber and there was a symbolic link from /opt/PackageName-VersionNumber to /opt/PackageName.

   - Configs go in /opt/PackageName/etc
   - Logs go in /opt/PackageName/logs
   - Binaries go in /opt/PackageName/bin
   - Data goes in /opt/PackageName/data

   - For apps that were to be distributed outside of our shop we wrote them to be relocatable by the package manager. This was a rare occurrence, and not "fun".

# How generate a public key from a private key
  - chmod 400 ~/.ssh/id_rsa
  - ssh-keygen -y -f ~/.ssh/id_rsa

# Check connection attempts and disable root login
    
    1 cd /var/log/
    2  ls
    3  tail -f messages
    4  less messages
    5  less secure
    6  vim /etc/ssh/sshd_config
    7  vi /etc/ssh/sshd_config
    8  sudo service sshd restart
    9  vi /etc/ssh/sshd_config
      - Set rootlogin to no
    10  systemctl status sshd
    11  iptables
    12  firewalld
    13  firewalld -l
    14  firewall-cmd --get-default-zone
    15  firewall-cmd --get-active-zones
    16  firewall-cmd -l
    17  sudo firewall-cmd --list-all

# How to write tools with python and linux

    1. Create a python project say in /opt/speech/my_tool with virtual environment /opt/speech/my_tool/venv
    2. Say the program is called /opt/speech/my_tool/thetool.py
    3. Create a file called "thetool" in the same directory with contents
       #! /opt/speech/my_tool/venv/bin/python
        from thetool import main
        main()
    4. sudo chmod +x /opt/speech/my_tool/thetool
    5. Now create a symbolic link at a bin directory with 
       ln -s /opt/speech/my_tool/thetool /opt/speech/bin/thetool
    6  Now add this to a ~/.bash_profile or ~/.bashrc
       export PATH=$PATH:/opt/speech/bin
    7. Don't touch or edit the link -- this will remove the sumbolic link
    
# Don't edit symbolic links on linux
   - This causes the link to vanish
   - Alway edit the original file

# How to find the difference between two files in linux
  - git diff file1 file2

# How to add cloud network drive (Azure) to Linux

### Install Azure cli
  ```
  sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

  sudo sh -c 'echo -e "[azure-cli]
  name=Azure CLI
  baseurl=https://packages.microsoft.com/yumrepos/azure-cli
  enabled=1
  gpgcheck=1
  gpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/azure-cli.repo'

  sudo yum install azure-cli
  ```
### Install cifs mount tool and nc

  ```
  sudo yum install cifs-utils 
  dnf install nmap # install nc
  ```

### Open port 445 for smb
  ```
  resourceGroupName="xyzDevRG"
  storageAccountName="xyzsharedrive"

  # This command assumes you have logged in with az login
  httpEndpoint=$(az storage account show \
      --resource-group $resourceGroupName \
      --name $storageAccountName \
      --query "primaryEndpoints.file" | tr -d '"')
  smbPath=$(echo $httpEndpoint | cut -c7-$(expr length $httpEndpoint))
  fileHost=$(echo $smbPath | tr -d "/")

  nc -zvw3 $fileHost 445
  ```
### Mount the drive
  ```
  resourceGroupName="xyzDevRG"
  storageAccountName="xyzsharedrive"
  fileShareName="xyzspesc"

  mntPath="/mnt/$storageAccountName/$fileShareName"
  sudo mkdir -p $mntPath

  httpEndpoint=$(az storage account show \
      --resource-group $resourceGroupName \
      --name $storageAccountName \
      --query "primaryEndpoints.file" | tr -d '"')
  smbPath=$(echo $httpEndpoint | cut -c7-$(expr length $httpEndpoint))$fileShareName

  storageAccountKey=$(az storage account keys list \
      --resource-group $resourceGroupName \
      --account-name $storageAccountName \
      --query "[0].value" | tr -d '"')

  #sudo mount -t cifs $smbPath $mntPath -o vers=3.0,username=$storageAccountName,password=$storageAccountKey,serverino
  sudo mount -t cifs $smbPath $mntPath -o vers=3.0,username=$storageAccountName,password=$storageAccountKey,dir_mode=0777,file_mode=0777,serverino
  ```
  
  In some situation if a Azure file share is not mountable from any VM, it could be the case that the VM has access restrictions configured outside the OS. For example, in some enterprise environments, VMs don't have a public ip with only a VPN access and a private ip configured. In such cases, one may need to configure or rather add a private endpoint for the (Azure) fileshare. This usually involves the creation of a custom DNS setting with a certain FQDN (abcstorage.blob.core.windows.net) and a private ip (172.89.65.15). 
  
### How to tar a directory with exceptions
  ```
  tar -zcvf mytars/a_dir.tgz a_adir --exclude=a_dir/unwanted_sub_dir
  ```
  

### To list listening ports

  ```
  netstat -tulpn | grep LISTEN
  ```

### If ```supervisorctl status``` doesn't work 
  
  In a non-vscode terminal
  ```
  sudo service supervisord restart
  sudo /usr/local/bin/supervisorctl status
  ```
  
### How to sync two directories using rsync

  Make sure rsync is installed on all the machines

  For dry run 
  ```
  # If you are permissions or set date error use flags --no-perms --omit-dir-times
  # pay attention: rsync adir/ adir. Contents of adir (local) in adir (remote)
  rsync -anv --no-perms --omit-dir-times -e "ssh -i /home/speech/.ssh/Azure/id_rsa" /mnt/s_drive/adir/bdir/ speech@172.172.172.172:/mnt/speech/adir/bdir
  ```
  Else
  ```
  # If you are permissions or set date error use flags --no-perms --omit-dir-times
  # pay attention: rsync adir/ adir. Contents of adir (local) in adir (remote)
  sudo rsync -Pav --no-perms --omit-dir-times -e "ssh -i /home/speech/.ssh/Azure/id_rsa" /mnt/s_drive/adir/bdir/ speech@172.172.172.172:/mnt/speech/adir/bdir
  ```
  
  There are ways to run rsync in background.



# How to use openfortivpn as a background service

Add the service file /lib/systemd/system/openfortivpn@.service
    
    [Unit]
    Description=OpenFortiVPN for %I
    After=network-online.target
    Documentation=man:openfortivpn(1)

    [Service]
    Type=simple
    PrivateTmp=true
    ExecStart=/usr/bin/openfortivpn -c /etc/openfortivpn/%I.conf
    OOMScoreAdjust=-100

    [Install]
    WantedBy=multi-user.target
    
    
This was the recommendation or a service file used by a coworker that ensured that the service was last in priority to be killed gave permission for ppp because openfortivpn uses it. This setup somehow ensured that service did not die like it did with my setup.

    [Unit]
    Description=OpenFortiVPN
    After=network-online.target

    [Service]
    Type=simple
    PrivateTmp=true
    ExecStart=/usr/bin/openfortivpn -c /etc/openfortivpn/%I.conf
    OOMScoreAdjust=-1000

    DeviceAllow=/dev/ppp
    # Auto restart when it gets disconnected.
    Restart=always
    RestartSec=500ms

    [Install]
    WantedBy=multi-user.target


Create a file at /etc/openfortivpn/prodserver1 with contents. (Not prodserver1.conf)
    
    
    host = 52.23.122.899
    port = 443
    username = dev1
    password = xagahaghaghaghgahhaD5
    trusted-cert = 4daghagahaaghaghagcd
    
    
Run as a service with 

    sudo systemctl start openfortivpn@prodserver1.service
    sudo systemctl status openfortivpn@prodserver1.service
    sudo systemctl stop openfortivpn@prodserver1.service


# Why a cron can fail

If you have tested something as a particular user and things work out fine when you run, the cron version can still fail. root and a particular user can have different environments. Debug with say

    sudo cat /var/log/cron | grep SSHException
    sudo cat /var/log/cron | grep some_identifier


In fact as best practice run it immediately by scheduling for the next immediate minute and 
        
    sudo tail -f /var/log/cron

This problem was notorious when connecting to another machine and root really doesn't know about a certain host and crontab is being run as root. This is fixed by adding something like this in your program or script

    client.set_missing_host_key_policy(paramiko.AutoAddPolicy())


# How to make service out of openvpn in centos 6 with init.d (Not relevant on newer OSes which use systemd)

To use the init.d based service manager in centos 6 system, rename your .ovpn files to say .conf and place them
in /etc/openvpn/. If you are running this a cli command then you can directly do something like 

    sudo openvpn /etc/openvpn/whatever.ovpn
    
Btw, the .ovpn file is really this: https://github.com/OpenVPN/openvpn/blob/master/sample/sample-config-files/client.conf
The extension .ovpn is a windows thing.

But if you are using the service script that comes with openvpn installation on centos 6, then you have to rename them 
as .conf because the script is looking for it. The script by Douglas Keller <doug@voidstar.dyndns.org>.

    for c in `/bin/ls *.conf 2>/dev/null`; do
        bn=${c%%.conf}
        if [ -f "$bn.sh" ]; then
            . ./$bn.sh
        fi
        rm -f $piddir/$bn.pid
        # Handle backward compatibility, see Red Hat Bugzilla ID #458594
        script_security=''
        if [ -z "$( grep '^[[:space:]]*script-security[[:space:]]' $c )" ]; then
            script_security="--script-security 2"
        fi
        $openvpn --daemon --writepid $piddir/$bn.pid --cd $work --config $c $script_security
        if [ $? = 0 ]; then
            successes=1
        else
            errors=1
        fi
    done
        
Ok rename the file as whatever.conf and place it in /etc/openvpn/
Now do 

    ps -ef | grep vpn

Kill all existing connections

Now you can do service operations like 
    
    sudo service opnevpn start|stop|restart|condrestart|reload|reopen|status

eg 

    sudo service openvpn start
    sudo service openvpn stop
    sudo service openvpn restart
    
# How to get the ip of a machine on all vpn connections

    ifconfig | grep "inet addr" | cut -d':' -f2 | cut -d' ' -f1 | grep -v '10.0.200.9\|127.0.0.1'
    
where 10.0.200.9 is the ip of the machine on the local network. List all that you are not looking for. 

# How to strip away a token such as " or ,

    grep blab blah.txt | tr -d '"' 
    grep blab blah.txt | tr -d ','
    
# How to loop through all the files in a directory in bash

    FILES=/mnt/speech/*.py
    for f in $FILES
    do
      echo "Processing $f file..."
      cat $f
    done
    
# How to read line by line in bash

    input="afile.txt"
    while IFS= read -r line
    do
      echo "$line"
      echo "###########################################################################"
    done < "$input"
   
# How to write functions in bash

    function fetch {
      echo "Call to $1"
    }
    fetch John
# How to assign results to variable in bash

    project=`grep project agent.json | cut -d':' -f2 | cut -d',' -f1 | tr -d '"' | tr -d ' '`
    echo $project
    
# How to copy big or many files from remote Linux machine (on a choppy connection) to local Windows using WSL
    
    cp /mnt/c/Users/Bharath/.ssh/azure/id_rsa /home/bharath/.ssh/azure/id_rsa # wsl quirk 
    sudo chmod 600 /home/bharath/.ssh/azure/id_rsa # wsl quirk of not being able to change permission on /mnt/c/Users/Bharath
    
    dstdir=/mnt/speech/dst
    srcdir=/mnt/speech/src
    
    # if you are using a private identity file
    rsync -av --partial -e "ssh -i /home/bharath/.ssh/azure/id_rsa" speech@172.33.111.10:$srcdir $dstdir
    
# How to convert audio using sox in bash

    function convert {
      #echo "Convert to $1 and put in $2"
      sox.exe "$1" -e signed-integer -c 1 -b 16 -r 16000 "$2/$1" # convert to `16khz 16 bit pcm

    }
    srcfiles=first_names_rising/*.wav
    dst_dir=first_names_rising_pcm
    for srcfilename in $srcfiles
    do 
       convert $srcfilename $dst_dir
    done

# How to have ruby installed on Centos *

    gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
    \curl -sSL https://get.rvm.io | bash -s stable
    source /home/theatro/.profile
    rvm install ruby
    gem install jekyll
    jekyll -v
    gem install bundler
    bundle
    bundle exec jekyll serve
# Where are wsl2 backend docker images in Windows

    C:\Users\Bharath\AppData\Local\Docker\wsl\data
