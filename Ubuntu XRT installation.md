# Installing the XRT 2018.3 RC3 Patch 2 on a Ubuntu AWS AMI
First launch an Ubuntu AWS AMI from here: https://aws.amazon.com/marketplace/pp/B07CQ33QKV

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
git clone http://www.github.com/aws/aws-fpga.git $AWS_FPGA_REPO_DIR
cd $AWS_FPGA_REPO_DIR
source sdaccel_setup.sh #dont bother about the error that the XILINX_SDK variable is not set.
mkdir $SDACCEL_DIR/Runtime
cd $SDACCEL_DIR/Runtime
export XRT_PATH="${SDACCEL_DIR}/Runtime/XRT_2018.3.3.2"
git clone http://www.github.com/Xilinx/XRT.git -b 2018.3.3.2 ${XRT_PATH}
cd ${XRT_PATH}
sudo ./src/runtime_src/tools/scripts/xrtdeps.sh
cd build
./build.sh
cd Release
apt install --reinstall ./xrt_201830.2.1.0_18.04-xrt.deb
apt install --reinstall ./xrt_201830.2.1.0_18.04-aws.deb
````
