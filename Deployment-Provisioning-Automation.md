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
> 1. The resources Section is Mandatory: Resources is the only mandatory section of the CloudFormation template
> 2. The Transform Section is for Referencing Addiotional Code: The transform section may be used to reference additional code stored in S3, allowing for code re-use, such as template snippets/reusable pieces of CloudFormation code, or Lambda code
- **Infraestructure As Code**: CloudFormation allows yout to manage, configure, and provision AWS infraestucture as YAML or JSON code
- **Parameters**: Input custom values
- **Conditions**: Provision resources based on environment
- **Resources**: This section is mandatory and describes the AWS resources that CloudFormation will create
- **Mappings**: Allows you to create custom mappings like Region-AMI
- **Transform**: Allows you to reference code located in S3, Lambda code or reusable snippets of CloudFormation code

## Provisioning AWS Resources Using CloudFomrmation
**YAML or JSON template** -> YAML or JSON templates are used to describe the end state of the infrastructure you are either provisioning or changing
**S3** -> After creating the template, you upload it to CloudFormation using S3
**API Calls** -> CloudFormation reads the template and makes the API calls on your behalf 
**CloudFormation Stack** -> The resulting set of resources that CloudFormation builds from your template is called a stack

## Troubleshooting CloudFormation
 Use the CloudFormation console to view the status of your stack and error messages
 - **Insufficient permissions** -> Add permissions for the resources you are trying to create, delete, or modify
 - **Resource Limit Exceeded** -> Request a limit increase or delete unnecesary resources and retry
 - **UPDATE_ROLLBACK_FAILED** -> Fix the causing the failure and retry

## CloudFormation Demos
Check the CloudFormation console for **error messages** when troubleshooting

## CloudFormation StackSets
Create, delete, and update your CloudFormation stacks across multiple AWS accounts and regions using a single operation

Cross account roles -> For the Administrator account, use ``AWSCloudFormationStackSetAdministrationRole``, which is allowed to assume ``AWSCloudFormationStackSetExecutionRole`` to provision resources in the target accounts

## CloudFormation Best pratices
1. **IAM** Control access to CloudFormation using IAM
2. **Service Limit** If you hit a limit, CloudFormation will fail to create your stack
3. **Avoid Manual Updates** Manual updates cause errors when you try to update or delete the stack
4. **Use CloudTrail** Use CloudTrail to track all changes, along with two made them
5. **Use a Stack Policy** Protect critical stack resources from unintentional updates and mistakes caused by human error

## Exploring Blue / Green Deployments
1. Low risk Deployment Strategy
2. Enables Testing
3. Rollback is Fast and Easy

## Understanding Rolling Deployments
- **Batchers** -> Deploy new application versions and other changes in batches
- **Cost effective** -> You can set the batch size and the minimun number of servers to keep in service
- **Complexity** -> Mixed environment. Rolling back involves a re-deployment

## When to use canary Deployments
_Remember the canary in the coal mine_
An **early warning system** that can indicate that something is wrong in your application
- **DEPLOY** -> Deploy the new version to a small number of servers
- **DIRECT** -> Direct a small proportion of customer traffic to the new version, e.g., 10%
- **ENABLES CANARY TESTING** -> Test your application with a small proportion of real customers before you roll out to everybody

## Automating task using AWS Systems Manager
1. Managment tool
2. Inventory of your EC2 instances
3. Automation
**Visibility and control** of your AWS infraestructure of your AWS infrastructure at scale
- **INVENTORY AND AUTOMATION** -> Organize your inventory and logically group resources together, allowing you to automate boring task 
- **RUN COMMAND** -> Perform common operational tasks on groups of instances simultaneosly without logging in to each one
- **PATCH MANAGER** -> Patch multiple EC2 instances simultaneoly

## Implementing automating patching using AWS systems manager
_We can use AWS Systems Manager to pach multiple EC2 instances simultaneously_ -> Patch manager

## Using AWS Systems Manager EC2 Run Command
You can use EC2 Run Command to securely run a command on multiple EC2 instances simultaneosly without logging in to each one. SSH, RDP, or a bastion host is not required

## Understanding OpsWorks
A configuration management service allowing you to centrally manage the Operating System and/or application configurations on EC2 and on-premise systems
- **Automated configuration management service**, compatible with Puppet and Chef
- **CONFIGURATION AS CODE** -> Operating systmes, application settings, packages
- **MANAGED SERVICE** -> Managed instances of Puppet or Chef
- **OPSWORKS STACKS** -> Models your application as a stack consisting of multiple layersm e.g. database, web server, application, load balancer, etc.
