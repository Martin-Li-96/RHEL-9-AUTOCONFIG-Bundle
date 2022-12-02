# RHEL-9-AUTOCONFIG-Bundle
# Nvidia Driver for RHEL 9 with DKMS
1. Enable EPEL for RHEL 9
    - subscription-manager repos --enable codeready-builder-for-rhel-9-$(arch)-rpms
    - sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
2. Install DKMS
    - yum update -y
    - sudo yum install kernel-debug-devel dkms -y   
3. Download and Install Nvidia driver for RHEL 9 
    -



