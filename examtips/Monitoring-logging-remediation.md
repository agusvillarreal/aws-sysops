## Introduction to CloudWatch
Amazon CloudWatch is a monitoring service to monitor the **health and performance** of your AWS resources, 
as well as the applications you run on AWS, and in your own data center.
Default EC2 host-level metrics: CPU, network, disk, and status check
Use the CloudWatch agent for operating system-level metrics like memory usage, processes, and CPU idle time

## Creating CloudWatch Dashboard
Customize your view
- **A CUSTOM VIEW** -> Create a dashboard which monitors the metrics that are meaningful to you, e.g. all your production systems
- **MULTI-REGION** -> Display metrics for any Region. Change the Region and add the widget you want
- **REMEMBER TO SAVE!** -> CloudWatch doesn't automatically save your dashboard, so remember to save it once you have configured it 

## Exploring CloudWatch Logs
1. Log Events: Event **message** and **time stamp**, e.g. events from an Apache log
2. Log Stream: Sequence of log **events** from the **same source**, e.g. an Apache log coming from a specific EC2 instance. Each log stream must belong to a log group
3. Log Group: A group **streams** that share the **same retention**, monitoring, and access control settings, e.g. multiple web servers running Apache

## Collecting Metrics and Logs Using the CloudWatch Agent
- CloudWatch Agent: Sends operating system-level metrics and logs to CloudWatch
- Operating System Metrics: e.g. CPU, memory, swap, and disk usage
- Logs: e.g. `/var/log/messages` and application log files `var/log/httpd/access_log`

## Creating CloudWatch metric filters
how to configure metrci filters
Metric FIlter -> Filter for specific phrases in your logs, e.g., warnings, errors, or HTTP status codes
Log Group -> Configure metric filters for any **log group**
Use Case -> Receive **CloudWatch alerts** for specific errors, warnings, or mesages in your log files 

## Exploring CloudWatch Logs Insights
- **Query and Analyze** -> We can use CloudWatch Logs Insights to interactively query and analyze data stored in CloudWatch Logs
- **Visualizations** -> Query the logs directly and also generate visualizations, e.g., bar graph, line graph, pie chart, etc
- **Examples** -> Displays the 25 most recently added log events

## Receiving Notifications with CloudWatch
1. CloudWatch Alarms: You can create an alarm to **monitor any** CloudWatch **metric** in your account and alter you if a threshol is reached
2. Use case: An alarm that is triggered if CPU utilization **exceeds 90%** on your EC2 instance for **more than 5 minutes**
3. Integrated with SNS: Send an SNS notification which can **send an email** to your support team

### Service Quotas
- Monitor Service Quotas: CloudWatch can be used to monitor your service quotas / limits and notify you if you are about to reach the limit
- SNS Notificacion: An SNS notification can be used to email your IT support team
- Alerts: CloudWatch Alarms can alert you in the dashboard
- Use Case: You have reached 90% of the quota value for on-demand EC2 instances 

### Health events
AWS Health: Provides information about **changes in the health** of AWS resources -> EventBridge: **Can send health events** to EventBridge (was CloudWatch) -> CloudWatch Alarm: ...**triggerin** a CloudWatch -> Trigger an Action: Send an SNS notification, send a message to sqs, or trigger a Lambda function 

## Creating CloudWatch
- Alarm -> You can use an alarm to notify you when a threshold is hit 
- Use Case -> You can create an alarm for any metric, e.g. `CPUUtilization`
- Email Notifications -> CloudWatch can also send you email notifications using an SNS topic

## Introduction to CloudTrail
Records user activity in your AWS account
Records events related to **creation, modification, or deletion of resources** (such as IAM users, S3 buckets, and EC2 instances)
CloudTrail logs all the API calls that are made in yout AWS account
1. Audit Trail: **Who**, **when**, **what**, **where**, source IP, request parameters, and response
2. Use cases: **Incident investigation**, near real-time security analysis of user activity, and compliance
3. S3: **90 days history by default** Create your own trail in the console to store CloudTrail logs indefinitely in S3 

## Working with CloudTrail
- Enable by Default -> 90 day's event history
- Your Own Trail -> Create your own trail to store the data for more than 90 days
- Encrypted -> By default, when you create a trail in the console, the dara is encrypted and stored in S3

## AWS Config 101
Continously monitors the configuration of your AWS resources
- Configurarion Monitoring -> Continuosly monitors the configuration of your AWS resources for compliance, with a desired state that you define
- Dashboard -> Provides an inventory, and shows compliance and non-compliance
- Rules -> Define the desired state of your resource configuration, e.g.: 
	- `s3-bucket-public-read-prohibited`
	- `cloud-trail-encryption-enables`
	- `ec2-ebss-encryption-by-default`
- Conformance Packs -> A set of rules managed as one, e.g. Operational Best Practices for S3, EC2, IAM, etc.
- Automati  Remediation -> Remediates non-compliant resources by triggering an action that you define, e.g. stop or terminate a non-compliant instances

## Using AWS Config
Continuosly monitors the configuration of your AWS resources
- Monitors Configuration: Monitors the configuration of your AWS resources for compliance, with a desired state that you define
- Rules Define: The desired configuration state of your resources
- Dashboard Inventory: Provides an inventory, and shows compliance and non-compliance
