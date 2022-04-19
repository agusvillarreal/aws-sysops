#examtips 

## EC2 
EC2 is like a virtual machine (VM), hosted in AWS instead of your own data center
- Select the capacity that you need right now 
- Grow and shrink when you need
- Pay what you use
- Wait minutes

## Elastic Load Balancer
 | gp2 General Purpose SSD                          | io1 Provisioned IOPS SSD                                      | io2 Provisioned IOPS SSD |
 | ------------------------------------------------ | ------------------------------------------------------------- | ------------------------ |
 | Suitable for boot disks and general applications | Suitable for OLTP and latency-sensitive applications          |   Suitable for OLTP and latency-sensitive applications                          |
 | Up to 16,000 IOPS per volume                     | 50 IOPS / GiB // Up to 64,000 IOPS per volume                 |   500 IOPS / GiB // Up to 64,000 IOPS per volume                       |
 | Up to 99.9% durability                           | High performance and most expensive // UP to 99.9% durability |   Latest generation Provisioned IOPS volume // UP to 99.999% durability                     |

## Bastion Host
1. Conenct to Private Instances: A bastion host enables you to connect to private instances in your VPC from an untrusted network using **SSH** or **RDP**
2. Public Subnet: A **bastion** is in a public subnet and is reachable from the instances
3. Security Groups: You need to configure the security group associated with the **private subnet** to enable SSH / RDP access from the bastion 

## Elastic Load Balancer
- **Application Load Balancer**: HTTP/HTTPS. Inteligent load balancing. Routes request to a specific web server based on the type of request
- **Network Load Balancers**: Provides high-performance load balancing for TCP traffic
- **Classic Load Balancers**: The legacy optio that supports both HTTP/HTTPS and TCP. May still appear in the exam
- **Gateway Load Balancers**: Provides load balancing for third-party virtual appliances, like firewalls, Intrusion Detection and Preventions Systems
-  **X-Forwarded-For**: If you need the IPv4 address of your end user, look for the _X-Forwarded-For_ header

## Load Balancer Error Messages
Client-Side or Server-Side?
4xx is a client-side error
5xx is a server-side error

#### HTTP 504 - Gateway Timeout
The Elastic Load balancer could not establish a connection to the target
**Something is wrong with your application. Identify where the application is failing and fix the problem**

#### HTTP 502 - Bad Gateway
The target host is unreachable
**Check that your security groups allow traffic from the load balancer to the target hosts**

#### HTTP 503 - Unavailable
No registered targets
**Check you have targets registered**

## CloudWatch Metrics Exam Tips
- HealthyHostCount
- UnHelathyHostCount
- RequestCount

## Elastic Load Balancer Access Logs
- **Access Logs** -> Capture information relating to incoming request to your Elastic Load Balancer
- **Disabled by Default** -> You will nedd to enable access logs
- **Stored in S3** -> Encrypted and stored in a S3 bucket and decrypted when you access them

## Understanding sticky sessions
1. **Sitcky sesions override the Algorithm** -> Use cookies to identify a **session**, and send request that are part of the same session to the **same target** 
2. **When to use sitcky session** -> Applications that **cache session information locally** on the web server
3. **Use cases** -> Consider **shopping carts, online forms, a training website** - we don't want to log out our customers halfway through a task

## Disovering EC2 image builder
1. Automates the process of creating and maintaining AMI and Container images
2. 4 step process: Select a base OS image, customize by adding softwarre, test, and distribute to your chosen region
3. Terminology:
	- Image pipeline: Settings and process
	- Image recipe: Source image and build components
	- Build components: The software to include

1. Base OS: Provide a base OS image. e.g. Amazon Linux 2 AMI
2. Software: Define software to install e.g. .Net, Node.js, Python, latest security updates, latest kernel, security settigns
3. Test: Run test on the nwe image. e.g. does it boot correctly
4. Distribute: Distribute the image to the regions of your choice you are operating in

## Introduction to CloudFormation
Manage, configure, and provison your AWS infrastructure as a Code
> CloudFormation Template example
```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: "Template to create an EC2 instance"
Metadata:
	Instances:
	 Description: "Web Server Instance"

Parameters: 	#input values
	EnvType:
	  Description: "Environment type"
	  Type: String
	  AllowedValues:
	  	- prod
		- test

Conditions:
  CreateProdResources: !Equal [ !Ref EnvType, prod ]
  Mappings:		#e.g. set values based on a region
  RegionMap:
  	 eu-west-1:
	  "ami": "ami-0bdb1d6c15a40392c"

Mappings:		#e.g. set values based on a region
 RegionMap:
 	eu-west-1:
	 "ami": "ami-0bdb1d6c15a40392c"
	 
Transform: 		# include snippets of code ouside the main template
  Name: 'AWS::Include'
  Parameters:
    Location: 's3://MyBucketName/MyFileName.yaml'
	
Resources: # the AWS resources you are deploying
  EC2Instance:
    Type: AWS::EC2::Instance
	Properties:
	Outputs:
  InstanceID:
  	Description: The Instance ID
	Value: !Ref EC2Instance
	  InstanceType: t3.micro
	  ImageId: ami-0bdb1d6c15a40392c
  Outputs:
InstanceID:
  Description: The instance ID
  Value: !Red EC2Instance
    InstanceType: t2.micro
	ImageId: ami-0bdb1d6c15a40392c
```


