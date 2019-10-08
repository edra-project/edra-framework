# Installing the XRT 2019.1_RC2 on a Ubuntu 18.04 AWS AMI
First launch an Ubuntu AWS AMI from the link below on a <b>f1.2xlarge</b> machine:
````
https://aws.amazon.com/marketplace/pp/B07C45M889
````
The AMI uses Ubuntu 16.04, so make sure to update it to 18.04 before proceeding to the next step.

# 1. Install latest packages
````
sudo apt-get update  
sudo apt-get upgrade
sudo apt-get install gcc
sudo apt-get install g++     
sudo apt-get install make
sudo apt-get install linux-headers-`uname -r`   
sudo apt-get install linux-libc-dev    
sudo apt-get install ocl-icd-dev ocl-icd-libopencl1 opencl-headers ocl-icd-opencl-dev
sudo mkdir -p /etc/OpenCL/vendors/
````

# 2. Install AWS CLI
````
sudo apt-get install python-pip
sudo pip install awscli --upgrade
aws configure
````

# 3. Install XRT
````
sudo su
apt-get install git 
git clone http://www.github.com/aws/aws-fpga.git $AWS_FPGA_REPO_DIR
export AWS_FPGA_REPO_DIR=/home/ubuntu/aws-fpga
export XRT_RELEASE_TAG=2019.1_RC2 # Substitute XRT_RELEASE_TAG from https://github.com/aws/aws-fpga/blob/master/SDAccel/docs/XRT_installation_instructions.md
cd $AWS_FPGA_REPO_DIR
source sdaccel_setup.sh #dont bother about the error that the XILINX_SDK variable is not set.
cd $SDACCEL_DIR/Runtime
export XRT_PATH="${SDACCEL_DIR}/Runtime/${XRT_RELEASE_TAG}"
git clone http://www.github.com/Xilinx/XRT.git -b ${XRT_RELEASE_TAG} ${XRT_PATH}
cd ${XRT_PATH}
./src/runtime_src/tools/scripts/xrtdeps.sh
cd build
./build.sh
cd Release
apt install --reinstall ./xrt_201910.2.2.0_18.04-xrt.deb
apt install --reinstall ./xrt_201910.2.2.0_18.04-aws.deb
````
