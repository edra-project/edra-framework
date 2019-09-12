# Run "hello world" XRT example on an AWS AMI with installed XRT

1. Connect to your Ubuntu AMI
2. sudo su
3. export AWS_FPGA_REPO_DIR=/home/ubuntu/aws-fpga/
4. export VIVADO_TOOL_VERSION=2018.3
5. export AWS_PLATFORM=$AWS_PLATFORM_DYNAMIC_5_0 
6. cd $AWS_FPGA_REPO_DIR/SDAccel/examples/aws/helloworld_ocl_runtime/2018.3/
7. source $AWS_FPGA_REPO_DIR/sdaccel_setup.sh
8. source $AWS_FPGA_REPO_DIR/sdaccel_runtime_setup.sh
9. fpga-clear-local-image -S0 (optional)
10. ./helloworld







