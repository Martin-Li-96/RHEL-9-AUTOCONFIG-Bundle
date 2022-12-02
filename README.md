# RHEL-9-AUTOCONFIG-Bundle
## Nvidia Driver for RHEL 9 with DKMS
1. Enable EPEL for RHEL 9
    - subscription-manager repos --enable codeready-builder-for-rhel-9-$(arch)-rpms
    - sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
2. Install DKMS
    - sudo yum update -y
    - sudo yum install kernel-debug-devel dkms -y   
3. Download and Install Nvidia driver for RHEL 9 
    1. Choosing correct Product Series and download the driver from [link](https://www.nvidia.com/download/index.aspx)
    2. **Changing into multi-user mode and install the driver** 
        - sudo systemctl set-default multi-user.target
        - sudo sh ./NV*.run\
        **(NOTE:If you are enable the nouveau, the nvidia driver installer will give you a warning at you first run it. And you should choose ok, and clike yes for disable the nouveau by nvidia installer, \
        and then you need run sudo dracut --force && reboot. After reboot, run the installer once more)**
        - sudo systemctl set-default graphical.target && reboot
        
## Install the CUDA TOOLKITS
1. Download the CUDA TOOLKITs from [link](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=RHEL&target_version=9)
2. sudo sh ./cuda*.run\
**(If you have installed Nvidia Driver, please uncheck the option of install Nvdia Dirver )**
## Install CUDNN
Follow the steps in the [link](https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html)\
**(Recommand using  Tar File Installation)**
## Install Node.js
[link](https://github.com/nodesource/distributions) useing RHEL 8 source.
## Install FFMPEG
    1.enable RPMfusion
            - sudo rpm -ivh https://mirrors.rpmfusion.org/free/el/rpmfusion-free-release-9.noarch.rpm
            - sudo rpm -ivh https://mirrors.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-9.noarch.rpm
    2. Install ffmpeg ffmpeg-devel
            - sudo yum install ffmpeg ffmpeg-devel -y
## Install Oracle JDK
    1. Uninstall the origin JDK
             process=`rpm -qa | grep java`
              for i in $process
              do
                rpm -e --nodeps $i
              done
    2. Download the Oracle JDK RPM Package from (https://www.oracle.com/ca-en/java/technologies/downloads/)
    3. sudo rpm -ivh ./jdk*.rpm
    3. Add JDK to the PATH (change the java version to yours) sudo gedit /etc/profile
            export JAVA_HOME=/usr/java/jdk-19
            export CLASSPATH=$CLASSPATH:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
            export PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$PATH:$HOMR/bin
    4. sudo source /etc/profile
## Install R and Rstudio-Server
    1. Enable the EPEL for RHEL 9 and then run
            sudo yum install R or sudo dnf install R
    2. Download Rstudio-Server Daily Builds, because there is not official version for RHEL 9\
    **(Note: The daily build version may have some bug when you use it independtly. Recommand use it with jupyterhub)**
    Download address: (https://dailies.rstudio.com)
    3. sudo yum install sqlite* -y
    4. sudo rpm -ivh ./Rstudio-*.rpm
        
## Install gcc toolkits
    sudo yum install gcc-* -y
## Install netcdf
    sudo yum install netcdf* -y
## Install Julia
    1. Download Julia from (https://julialang.org/downloads/)
    2. Recommand put Julia to /opt directory
        tar -zvxf ./julia*.gz
        sudo mv ./julia-1.8.3 /opt/julia
    3. Add julia to the PATH
        export PATH=$PATH:/opt/julia/bin
## Install Octave
    sudo yum install octave* -y
## Install RVM for all user    
    1. Install GPG keys:
        gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
    2. \curl -sSL https://get.rvm.io | sudo bash -s stable
    3. add the user who want to use rvm to rvm group
        usermod -a -G rvm demo
## Install GO
    sudo yum install GO
    **(Option: you can install go version manager (GVM)(https://github.com/moovweb/gvm))
## Install Lua & LuaJIT
    sudo yum install lua* luajit*
## Install Lisp (SBCL)
    1. Download SBCL source file from (https://sourceforge.net/projects/sbcl/files/sbcl/2.2.11/sbcl-2.2.11-x86-64-linux-binary.tar.bz2/download)
    2. tar -xvjf ./sbcl*.bz2
    3. cd ./sbcl-2.2.11*-linux
    4. sudo ./install.sh
    **(Option install quicklisp)**
            - https://www.quicklisp.org/beta/
    sudo gedit /etc/profile
    export PATH=$PATH:/usr/local/bin
## Install LaTex (Texlive)
    sudo yum install texlive* -y
## Install Spark
    1. Download Apache-Spark (https://spark.apache.org/downloads.html)
    2. tar -zvxf ./spark*.tgz
    3. sudo mv ./spark-*hadoop3 /opt/spark
    4. gedit /etc/profile
        export PATH=$PATH:/opt/spark/bin
## Install flatpack and flathub
    1. sudo yum install flatpak
    2. sudo flatpak remote-add flathub https://flathub.org/repo/flathub.flatpakrepo
## Install clojure
    1. curl -O https://download.clojure.org/install/linux-install-1.11.1.1200.sh
    2. sudo sh ./linux-install*.sh
## Install mysql
    1. Download mysql community server 
        https://dev.mysql.com/downloads/file/?id=511985
    2. sudo rpm -ivh mysql80-community-release-el9-1.noarch.rpm
    3. sudo yum install mysql-server
    4. sudo systemctl start mysqld
    5. sudo cat /var/log/mysqld.log | grep password (get the temp passwd)
    6. mysql_secure_installation (choose yes) and reset the root password
    **(Change the directory of database)**
            - sudo systemctl stop mysqld
            - sudo mkdir /data
            - chown -R mysql:mysql /data
            - sudo cp -Rp /var/lib/mysql/* /data
            - sudo cp /etc/my.cnf /etc/my.cnf.bak
            - sudo gedit /etc/my.cnf\
                    [mysqld]          
                    datadir=/data/mysql
                    socket=/data/mysql/mysql.sock
                    [client]:
                    port=3306
                    socket=/data/mysql/mysql.sock
     7. disable SELinux
        sudo gedit /etc/selinux/config
                SELINUX=disabled
     8. reboot
     9. sudo systemctl start mysqld
## Install PostgreSQL
    1. sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm
    2. sudo dnf -qy module disable postgresql
    3. sudo dnf install -y postgresql15-server
    4. sudo /usr/pgsql-15/bin/postgresql-15-setup initdb
    5. sudo systemctl enable postgresql-15
    6. sudo systemctl start postgresql-15
    **(Change database directory)**
    video https://www.youtube.com/watch?v=CQh10DTR8d8
            - mkdir /postgres
            - cp -Rp /var/lib/pgsql/15/data /postgres
            - chown -R postgres:postgres /postgres
            - chmod -R 750 /postgres
            gedit /var/lib/pgsql/15/data/postgresql.conf
                  - data_directory = '/postgres/data'
    # Run postgresql
            sudo -u postgres psql
   # Change Postgresql
            sudo -u postgres psql
            ALTER USER postgres PASSWORD 'myPassword';
            show SHOW hba_file;
            gedit /var/lib/pgsql/15/data/pg_hba.conf
            peer -> md5
         # Run postgres by:
            psql -U postgres -W  
            
      

                    

