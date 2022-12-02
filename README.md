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
    3. 
