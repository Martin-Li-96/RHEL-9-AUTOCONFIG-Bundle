# RHEL-9-AUTOCONFIG-Bundle
# Nvidia Driver for RHEL 9 with DKMS
1. Enable EPEL for RHEL 9
    - subscription-manager repos --enable codeready-builder-for-rhel-9-$(arch)-rpms
    - sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
2. Install DKMS
    - yum update -y
    - sudo yum install kernel-debug-devel dkms -y   
3. Download and Install Nvidia driver for RHEL 9 
    1. Choosing correct Product Series and download the driver from [link](https://www.nvidia.com/download/index.aspx)
    2. **Changing into multi-user mode and install the driver** 
        - sudo systemctl set-default multi-user.target
        - sudo sh ./NV*.run\
        **(NOTE:If you are enable the nouveau, the nvidia driver installer will give you a warning at you first run it. And you should choose ok, and clike yes for disable the nouveau by nvidia installer, \
        and then you need run sudo dracut --force && reboot. After reboot, run the installer once more)**
        - sudo systemctl set-default graphical.target && reboot
        
#Install the CUDA TOOLKITS
1. Download the CUDA TOOLKITs from [link](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=RHEL&target_version=9)
2. sudo sh ./cuda*.run\
**(If you have installed Nvidia Driver, please uncheck the option of install Nvdia Dirver )**
# Install CUDNN
Follow the steps in the [link](https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html)\
**(Recommand using  Tar File Installation)**

           



