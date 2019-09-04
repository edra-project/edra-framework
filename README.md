# Testing the EDRA Accelerator on an AWS F1 FPGA Instance

This document provides instructions on how to execute the provided FPGA-accelerated application based on DAE architecture on an Amazon AWS F1 instance. 

# Table of Content

1. [Demonstration Application Overview](#overview) 
2. [AWS Prerequisites](#prerequisites)
   * [AWS Account, F1/EC2 Instances, On-Premises, AWS IAM Permissions, AWS CLI and S3 Setup](#iss)
3. [Obtain application files and prepare the instance for execution of the accelerated application](#createapp)
4. [Run the FPGA accelerated application on F1](#runonf1)
5. [Contact](#read)


<a name="overview"></a>
# 1. EDRA Demonstration Application Overview

The application developed as a demonstrator for EDRA is based on the [RAxML application](https://github.com/stamatak/standard-RAxML). The application has been modified according to the Decoupled Access Execute paradigm developed within the EDRA project and an FPGA accelerator has been developed to perform the most demanding computational task. The FPGA-accelerated application is designed to be deployed on an Amazon F1 instance and for demonstration purposes it includes a small data set as well as a software component that is meant to verify that the results obtained be the original application and the accelerated one are equivalent.

The accelerated application is provided in binary form. The software components are the compiled source code running on the host processor and the FPGA accelerator is in the form of an AWS FPGA binary file (.awsxclbin) containing the FPGA bitstream and all other components required to execute the application. A small dataset is also provided for a sample run. Source code for the software application and the accelerator can be provided upon request.

To execute the accelerated application, access to an AWS F1 instance is required. The following steps describe the process to prepare a proper environment on AWS that can be used to deploy the application and the instructions to actually execute it on the AWS instance. In case you have already setup AWS accounts and deployed F1 instances, feel free to skip the initial steps and proceed directly to the execution of the application.

It should be mentioned that since the current repository provides the application in binary form and not source code, you are not required to setup a development environment nor obtain third party licenses. The demonstration application is provided for free from the the EDRA project, however have in mind that there are associated usage fees related to the use of EC2/F1 instances from Amazon. 

<a name="prerequisites"></a>
# Prerequisites
<a name="iss"></a>
## AWS Account, F1/EC2 Instances, AWS IAM Permissions, AWS CLI and S3 Setup (One-time Setup)
If you do not already have an AWS account, you should follow these steps to establish an account and setup a proper F1 instance for the demo application.

* [Setup an AWS Account](https://aws.amazon.com/free/)
* Login to your account and select the EC2 console
* Create the proper keys to access your instance. Please follow the instructions provided [here](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/ec2-api-permissions.html).
* Go back to the EC2 console and select Launch Instance.
  * Launch an instance using the FPGA Developer AMI, which comes pre-installed with SDAccel and required licenses.
  * The demonstration application has been developed using Xilinx tools version 2018.3, therefore be careful to select a compatible AMI version. It should be marked as <i>v1.6.0-v1.6.X (Xilinx Vivado/SDx 2018.3)</i>.
  * When selecting a region to deploy the instance, choose US East (N. Virginia).
  * Select the f1.2xlarge instance and keep the default configuration settings.
  * Before completing the process you will be asked to create a new key pair or use an existing one. Select a new key pair and save the file \*.pem file. You need to keep this file in a safe place as it is your "passport" to accessing remotely the AWS instance.
    * Important: if you are using a *nix based machine as your access environment, make sure that the \*.pem file has 600 permissions. 
  * Launch the instance. 
     * If this is the first time you launch an F1 instance, you will be prompted that you need to be granted permission to launch an EC2 f1.2xlarge instance (increase limit request to 1) in the specified region. The application process is simple and you will be granted the limit increase typically within a day from Amazon.  

<a name="createapp"></a>
# 2. Obtain application files and prepare the instance for execution of the accelerated application

* To obtain all required files for the demonstration application, clone this repository: 

   ```
       $ git clone https://github.com/kingmouf/EDRA_demo.git $EDRA_DEMO_DIR  
   ```
By cloning the repository you will find in the *edra_demo* folder the following files:
  * host : this is the application executable
  * dataEV.txt , dataLEFT.txt, dataRIGHT.txt ,  dataIn.txt and dataOut.txt : these are the data files required by the application
  * plfDNACore.hw.xilinx_aws-vu9p-f1-04261818_dynamic_5_0.awsxclbin : this is the binary file that contains the accelerator image (AWS FPGA binary file)

* In your EC2 console, you can see your instances and should you have launched successfully the F1 instance, it will be marked as *running*. You need to notice the Public DNS (IPv4) field, as this is going to provide the IP address that you need in order to SSH to the instance.

To connect to your instance you will need the .pem file that you have downloaded. You can connect as the user *centos* using SSH:

   ```
       $ ssh -i <.pem file> centos@<instance_ip>  
   ```
Once you have successfully established a connection, do the following:

   ```
       $ git clone https://github.com/aws/aws-fpga.git $AWS_FPGA_REPO_DIR
       $ cd $AWS_FPGA_REPO_DIR                                         
       $ source sdaccel_setup.sh
       $ sudo sh
       $ export VIVADO_TOOL_VERSION=2018.3
       $ source ./sdaccel_runtime_setup.sh
   ```

Back in your local machine, you need to transfer the application executable, the required data files and the .awsxclbin file. This can be done using Secure Copy. More specifically, the following will copy all required files to the home directory of the instance:

   ```
       $ cd $EDRA_DEMO_DIR/edra_demo
       $ scp -i <.pem file> plfDNACore.hw.xilinx_aws-vu9p-f1-04261818_dynamic_5_0.awsxclbin centos@<instance_ip>:~
       $ scp -i <.pem file> host centos@<instance_ip>:~
       $ scp -i <.pem file> data*.txt centos@<instance_ip>:~
   ```

Now everything is setup to run the demo application!

<a name="runonf1"></a>
# 3. Run the FPGA accelerated application on Amazon FPGA instances

On your F1 instance type:

   ```
       # ./host dataEV.txt dataLEFT.txt dataRIGHT.txt dataIn.txt dataOut.txt 
   ```

The application will be executed both in software and using the accelerator. At the end, it compares the results of the two runs and prints a message on the terminal if they are equivalent.

This completes the demo!

* Note: it has been observed that on certain occassions depending on the setup environment and the files contained in the AWS-FPGA repo, that errors may be reported. This is not caused by the application but it is an SDAccel issue. If an error arises, you will need to add to your path the *sdaccel.ini* file. This can be found in several locations (one example is the $AWS_FPGA_REPO_DIR/SDAccel/examples/aws/helloworld_ocl_runtime/sdaccel.ini )

<a name="read"></a>
# 4. Contact

For any issues, please contact us: 

Dimitris Theodoropoulos (dtheodoropoulos@mhl.tuc.gr)

Nikos Alachiotis (nalachiotis@mhl.tuc.gr)

Andreas Brokalakis (abrokalakis@mhl.tuc.gr)

