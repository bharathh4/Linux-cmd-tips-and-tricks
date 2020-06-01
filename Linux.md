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
  


