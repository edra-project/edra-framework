# Run "hello world" XRT example on an AWS AMI with XRT installed

Connect to your Ubuntu AMI and then follow these steps:
````
sudo su
export AWS_FPGA_REPO_DIR=/home/ubuntu/aws-fpga/
export VIVADO_TOOL_VERSION=2019.1
export AWS_PLATFORM=$AWS_PLATFORM_DYNAMIC_5_0 
cd $AWS_FPGA_REPO_DIR/SDAccel/examples/aws/helloworld_ocl_runtime/2018.3_2019.1/
source $AWS_FPGA_REPO_DIR/sdaccel_setup.sh #dont bother about the error that the XILINX_SDK variable is not set.
source $AWS_FPGA_REPO_DIR/sdaccel_runtime_setup.sh
fpga-clear-local-image -S0 #(optional)
./helloworld







