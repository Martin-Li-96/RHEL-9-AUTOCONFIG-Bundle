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
    # Change Postgresql passwd
            sudo -u postgres psql
            ALTER USER postgres PASSWORD 'myPassword';
            show SHOW hba_file;
            gedit /var/lib/pgsql/15/data/pg_hba.conf
            peer -> md5
         # Run postgres by:
            psql -U postgres -W  
            
## Code-server
    sudo curl -fsSL https://code-server.dev/install.sh | sh
## Scala
    Download: https://github.com/lampepfl/dotty/releases/download/3.2.1/scala3-3.2.1.tar.gz
    sudo tar -zvxf ./scala*.gz
    sudo cp -r ./scala*.1 /opt/scala
    sudo gedit /etc/profile
    export PATH=$PATH:/opt/scala/bin
## Kotlin 
    Download: https://github.com/JetBrains/kotlin/releases/download/v1.7.21/kotlin-native-linux-x86_64-1.7.21.tar.gz
    sudo tar -zvxf ./kotlin*.gz
    sudo cp -r ./kotlin*.21 /opt/kotlin
    sudo gedit /etc/profile
    export PATH=$PATH:/opt/kotlin/bin
    
## Anaconda && Jupyterhub
    1. Download Anaconda: https://www.anaconda.com/products/distribution
    2. sudo sh ./A*.sh
    3. Set install path = /opt/anaconda3
    4. open a new terminal
    5. sudo su
    6. conda update conda -y
    7. conda create -n jupyterhub --clone base
    0. conda activate jupyterhub
    9. pip install jupyterlab jupyterhub
    10. npm install -g configurable-http-proxy
    11. pip install ipywidgets
    12. jupyter labextension install @jupyterlab/hub-extension
    13. pip install jupyter-server-proxy
    14. pip install jupyter-vscode-proxy
    15. pip install jupyter-rsession-proxy
    16. pip install jupyter-c-kernel
    17. install_c_kernel
    18. cp -r /root/.local/share/jupyter/kernels/c /opt/anaconda3/envs/jupyterhub/share/jupyter/kernels/c
    19. conda install octave_kernel -c conda-forge -y
    20. conda install texinfo -y
    21. python -m octave_kernel install
    22. conda create -n C++ -y
    23. conda activate C++
    24. conda install conda -y
    25. pip install ipykernel
    26. conda install xeus-cling -c conda-forge -y
    27. cp -r /opt/anaconda3/envs/C++/share/jupyter/kernels/xcpp11 /opt/anaconda3/envs/jupyterhub/share/jupyter/kernels/xcpp11
    28. cp -r /opt/anaconda3/envs/C++/share/jupyter/kernels/xcpp14 /opt/anaconda3/envs/jupyterhub/share/jupyter/kernels/xcpp14
    29. cp -r /opt/anaconda3/envs/C++/share/jupyter/kernels/xcpp17 /opt/anaconda3/envs/jupyterhub/share/jupyter/kernels/xcpp17
    30. conda create -n fortran -y
    31. conda activate fortran 
    32. conda install conda -y
    33. pip install ipykernel
    34. conda install lfortran -c conda-forge -y
    35. cp -r /opt/anaconda3/envs/fortran/share/jupyter/kernels/fortran /opt/anaconda3/envs/jupyterhub/share/jupyter/kernels/fortran
    36. conda create -n java -y
    37. conda activate java
    38. conda install conda -y 
    39. pip install ipykernel
    40. wget -P /opt/ijava_zip https://github.com/SpencerPark/IJava/releases/download/v1.3.0/ijava-1.3.0.zip
    41. unzip /opt/ijava_zip/ijava-1.3.0.zip -d /opt/ijava
    42. python3 /opt/ijava/install.py --sys-prefix
    43. cp -r /opt/anaconda3/envs/java/share/jupyter/kernels/java /opt/anaconda3/envs/jupyterhub/share/jupyter/kernels/java
    44. conda activate jupyterhub
    45. npm install uuid@latest
    46. npm install -g ijavascript
    47. ijsinstall
    48. cp -r /root/.local/share/jupyter/kernels/javascript /opt/anaconda3/envs/jupyterhub/share/jupyter/kernels/javascript
    49. conda create -n ssh -y
    50. conda activate ssh
    51. conda install conda -y
    52. pip install ipykernel
    53. pip install -U sshkernel
    54. python -m sshkernel install --sys-prefix
    55. cp -r /opt/anaconda3/envs/ssh/share/jupyter/kernels/ssh /opt/anaconda3/envs/jupyterhub/share/jupyter/kernels/ssh
    56.  R
    57. install.packages('IRkernel',repos='https://cran.r-project.org')
    58. IRkernel::installspec()
    59. IRkernel::installspec(user = FALSE)
    60. q()
    61. cp -r /root/.local/share/jupyter/kernels/ir /opt/anaconda3/envs/jupyterhub/share/jupyter/kernels/ir
    62. yum install ruby* zeromq-devel
    63. gem install iruby
    64. iruby register --force
    65. cp -r /root/.local/share/jupyter/kernels/ruby /opt/anaconda3/envs/jupyterhub/share/jupyter/kernels/ruby
    66. conda create -n bash -y
    67. conda activate bash
    68. conda install conda -y
    69. pip install ipykernel
    70. pip install bash_kernel
    71. python -m bash_kernel.install    
    72. User install Ijulia by them selfs: Terminal->julia->]->add IJulia
    73. Go kernel install by user:  https://github.com/gopherdata/gophernotes
    74.conda create -y -n beakerx 'python>=3'
    75.conda install -y -c conda-forge ipywidgets beakerx
    76. Other kernel need user install by themselfs: https://github.com/jupyter/jupyter/wiki/jupyter-kernels
##  Install TexLive
    Download install-tl.sh:
        - https://tug.org/texlive/acquire-netinstall.html 
        - https://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz
    tar -zvxf ./*.gz
    cd ./install-tl-unx/install*
    sudo ./install-tl
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    

                    

