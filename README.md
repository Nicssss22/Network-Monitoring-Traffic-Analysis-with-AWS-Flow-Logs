<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Monitoring with Flow Logs

**Project Link:** [View Project](https://nextwork.ai/projects/6954a600-a89e-56b0-886b-7054da118a3b)

**Author:** agustinnico2302@gmail.com  
**Email:** agustinnico2302@gmail.com

---

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/6954a600-a89e-56b0-886b-7054da118a3b_3e1e79a1)

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC allows you to build a private network for your AWS resources. You can organize resources into subnets, control access, and manage how traffic moves through the network.

### How I used Amazon VPC in this project

In today’s project, I used Amazon VPC to create flow logs so I could monitor network traffic to and from resources in the VPC.

### One thing I didn't expect in this project was...

One thing I didn't expect in this project was the log insights in CloudWatch, because I facsinated that I could create queries on my own or tell AI what query do I need.  I just need to review and accept the generated query rather to create it from the scratch.

### This project took me...

This project took me for about 2-3 hours because the interface of the AWS Management Console has been updated compared to what's being being though here in NextWork. But I get along with it by just exploring the CloudWatch sidebar.

## In the first part of my project...

### Step 1 - Set up VPCs

In this step, I will use VPC wizard to setup my VPC in minutes rather then doing the manual setup. 

### Step 2 - Launch EC2 instances

In this step, I will launch EC2 instance in the public subnet of my both VPCs.

### Step 3 - Set up Logs

In this step, I will set up VPC Flow Logs to track inbound and outbound traffic, and create a log group for the log records.

### Step 4 - Set IAM permissions for Logs

In this step, I will create an IAM role to give permission to VPC flow logs to write logs and send them to CloudWatch. With this, I can finish setting up my subnet's flow log.

## Multi-VPC Architecture

I started my project by launching 2 VPCs that are composed of one (1) public subnet each.

The CIDR blocks for VPCs 1 and 2 are difference with each other. They have to be unique because VPC peering doesn't allow overlapping CIDR blocks for both VPC, since it will cause connectivity issues.

### I also launched EC2 instances in each subnet

My EC2 instances’ security groups allow SSH traffic from any source, so I can connect to my instances using EC2 Instance Connect. I also enabled ICMP traffic from any source so I can ping my instances and test connectivity.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/6954a600-a89e-56b0-886b-7054da118a3b_e7fa8775)

## Logs

Logs are like a diary of computer systems. It contains the records that happens such as users logging in and the errors popping up. It is a great record to see the activities happening in a system in order to debug problems and to track something according on the purpose.

Log groups are the storage of CloudWatch logs to keep related logs together. It is region specific, means that the logs only saved where it was created. Although there are other workarounds such as bringing logs together from different regions. 

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/6954a600-a89e-56b0-886b-7054da118a3b_e8398869)

## IAM Policy and Roles

I created an IAM policy that allows VPC Flow Logs to create log groups and streams, send log data to CloudWatch, and view information about them. I will attach this policy to an IAM role, then assign the role to my VPC Flow Logs configuration so it can perform these actions.

I also created an IAM role so I could attach the IAM policy to it and assign the role to the VPC Flow Logs service.

A custom trust policy is a part of IAM role that specifies who or what is allowed to use that role.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/6954a600-a89e-56b0-886b-7054da118a3b_4334d777)

## In the second part of my project...

### Step 5 - Ping testing and troubleshooting

In this step, I will test the connectivity between the instance 1 from VPC 1 and instance 2 from VPC 2 using the ICMP protocol.

### Step 6 - Set up a peering connection

In this step, I will configure the route tables for both VPCs to add routes through the peering connection.

### Step 7 - Analyze flow logs

In this step, I will review the flow logs about the VPC 1's public subnet and analyze to get insights.

## Connectivity troubleshooting

My first ping test between my EC2 instances had no replies, which means that the connection between the 2 EC2 instances has failed and signifies network problem.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/6954a600-a89e-56b0-886b-7054da118a3b_99d4ba42)

I could receive ping replies if I ran the ping test using the other instance's public IP address, which means the connectivity between the instance 1 and 2 is successful using the IPv4 address of instance 2.

## Connectivity troubleshooting

Looking at VPC 1's route table, I identified that the ping test with Instance 2's private address failed because the peering connection between the two VPC isn't included in the routing table.

### To solve this, I set up a peering connection between my VPCs

I also updated both VPCs’ route tables so that traffic between them can be routed using the private IPv4 addresses of their resources. And additionally, each VPC needs to know how to route their traffic to another VPC through peering connection.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/6954a600-a89e-56b0-886b-7054da118a3b_7316a13d)

## Connectivity troubleshooting

I received ping replies from Instance 2's private IP address! This means that the connection through peering connection between the resources (EC2 instance) of two (2) VPCs is successful.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/6954a600-a89e-56b0-886b-7054da118a3b_4ec7821f)

## Analyzing flow logs

version — Flow Logs record format version.
account-id — AWS account that owns the network interface.
interface-id — ID of the network interface being monitored.
srcaddr / dstaddr — Source and destination IP addresses.
srcport / dstport — Source and destination ports.
protocol — Network protocol number; 6 means TCP.
packets / bytes — Traffic volume observed.
start / end — Start and end times for the summarized traffic, as Unix timestamps.
action — Whether the traffic was ACCEPTed or REJECTed by the applicable network controls.
log-status — Whether the record was delivered normally (OK)

For example, the flow log I've captured tells us all the network traffic coming in and out of the specific ENI whether it is rejected or accepted.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/6954a600-a89e-56b0-886b-7054da118a3b_d116818e)

## Logs Insights

Logs Insights helps to search and analyze stored logs in CloudWatch by using queries in which is customizable according on your needs. It can be used to find patterns and summarize records from the stored logs.

The query finds to top 10 source-and-destination IP pairs that transfered the most data in log group over the last hour. Based from the screenshot it shows that the 	10.1.4.92 and 3.0.5.37 IP pair transfered over 42764 bytes of data in total over the last hour. The pair of 10.1.4.92 and 10.2.4.177 transfered the least amount of data.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/6954a600-a89e-56b0-886b-7054da118a3b_3e1e79a1)

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/6954a600-a89e-56b0-886b-7054da118a3b)*
