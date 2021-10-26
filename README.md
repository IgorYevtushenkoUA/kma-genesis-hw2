
# Homework

## Create VPC
> **aws ec2 create-vpc --cidr-block 10.10.0.0/18 --no-amazon-provided-ipv6-cidr-block --query Vpc.VpcId --output text**

```
vpc-08865727912919450
```

## Create 3 subnets within your VPC
> **aws ec2 create-subnet --vpc-id vpc-08865727912919450 --cidr-block 10.10.1.0/24**
```
{
	"Subnet": {
		"AvailabilityZone": "us-east-2c",
		"AvailabilityZoneId": "use2-az3",
		"AvailableIpAddressCount": 251,
		"CidrBlock": "10.10.1.0/24",
		"DefaultForAz": false,
		"MapPublicIpOnLaunch": false,
		"State": "available",
		"SubnetId": "subnet-0ba68314d8a034b7b",
		"VpcId": "vpc-08865727912919450",
		"OwnerId": "**************",
		"AssignIpv6AddressOnCreation": false,
		"Ipv6CidrBlockAssociationSet": [],
		"SubnetArn": "arn:aws:ec2:us-east-2:**************:subnet/subnet-0ba68314d8a034b7b"
	}
}
```
> **aws ec2 create-subnet --vpc-id vpc-08865727912919450 --cidr-block 10.10.2.0/24**
```
{
   "Subnet": {
       "AvailabilityZone": "us-east-2c",
       "AvailabilityZoneId": "use2-az3",
       "AvailableIpAddressCount": 251,
       "CidrBlock": "10.10.2.0/24",
       "DefaultForAz": false,
       "MapPublicIpOnLaunch": false,
       "State": "available",
       "SubnetId": "subnet-097ccaf34143ebe28",
       "VpcId": "vpc-08865727912919450",
       "OwnerId": "**************",
       "AssignIpv6AddressOnCreation": false,
       "Ipv6CidrBlockAssociationSet": [],
       "SubnetArn": "arn:aws:ec2:us-east-2:**************:subnet/subnet-097ccaf34143ebe28"
   }
}
```
> **aws ec2 create-subnet --vpc-id vpc-08865727912919450 --cidr-block 10.10.3.0/24**

```
{
   "Subnet": {
       "AvailabilityZone": "us-east-2c",
       "AvailabilityZoneId": "use2-az3",
       "AvailableIpAddressCount": 251,
       "CidrBlock": "10.10.3.0/24",
       "DefaultForAz": false,
       "MapPublicIpOnLaunch": false,
       "State": "available",
       "SubnetId": "subnet-07f2f411fda679a36",
       "VpcId": "vpc-08865727912919450",
       "OwnerId": "**************",
       "AssignIpv6AddressOnCreation": false,
       "Ipv6CidrBlockAssociationSet": [],
       "SubnetArn": "arn:aws:ec2:us-east-2:**************:subnet/subnet-07f2f411fda679a36"
   }
}
```

## Create AWS Autoscaling group (ASG)

### Create an ami
> **aws ec2 create-image --instance-id i-09a5f8a4f50b97588 --name ASGCLI**

```
{
   "ImageId": "ami-0f59c84a260a67f5c"
}
```
### Create launch configuration

> **aws autoscaling create-launch-configuration --launch-configuration-name my-lc2 --image-id ami-0f59c84a260a67f5c --instance-type t2.micro --metadata-options "HttpEndpoint=enabled,HttpTokens=required"**

### Create autocsaling group
> **aws autoscaling create-auto-scaling-group --auto-scaling-group-name my-asg --launch-configuration-name my-lc2 --min-size 1 --max-size 5 --vpc-zone-identifier subnet-0ba68314d8a034b7b**


## Create EC2 instance within ASG based on latest AMI Amazon Linux 2 with 15GiB attached EBS (Elastic block storage)

> aws ec2 run-instances --image-id ami-00047694623b25555 --count 1 --instance-type t3a.xlarge --tag-specifications 'ResourceType=instance, Tags=[{ Key=Name,Value=MyInstance1}]' --security-group-ids sg-04061a7e2e3446baf sg-0994939db764d17a6 sg-05cff46203964e587 --user-data file://d:/UserData.txt


## Add to your EC2 instance Security groups, that allows connection to TCP ports 22 (SSH), 80 (HTTP), 443 (HTTPS)

> **aws ec2 create-security-group --group-name secret-ssh --description "ssh (22)"**
``` 
{
   "GroupId": "sg-04061a7e2e3446baf"
} 
```

> **aws ec2 create-security-group --group-name secret-http --description "http (80)"**
```
{
   "GroupId": "sg-0994939db764d17a6"
}
```
> **aws ec2 create-security-group --group-name secret-https --description "https (443)"**
```
{
   "GroupId": "sg-05cff46203964e587"
}
```
> **aws ec2 authorize-security-group-ingress --group-id sg-04061a7e2e3446baf --protocol tcp --port 22 --cidr  0.0.0.0/0**
```
{
   "Return": true,
   "SecurityGroupRules": [
       {
           "SecurityGroupRuleId": "sgr-0029af4f48a0b9579",
           "GroupId": "sg-04061a7e2e3446baf",
           "GroupOwnerId": "**************",
           "IsEgress": false,
           "IpProtocol": "tcp",
           "FromPort": 22,
           "ToPort": 22,
           "CidrIpv4": "0.0.0.0/0"
       }
   ]
}
```
> **aws ec2 authorize-security-group-ingress --group-id sg-0994939db764d17a6 --protocol tcp --port 80 --cidr  0.0.0.0/0**
```
{
   "Return": true,
   "SecurityGroupRules": [
       {
           "SecurityGroupRuleId": "sgr-0ea2007df712be5da",
           "GroupId": "sg-0994939db764d17a6",
           "GroupOwnerId": "**************",
           "IsEgress": false,
           "IpProtocol": "tcp",
           "FromPort": 80,
           "ToPort": 80,
           "CidrIpv4": "0.0.0.0/0"
       }
   ]
}
```
> **aws ec2 authorize-security-group-ingress --group-id sg-05cff46203964e587 --protocol tcp --port 443 --cidr  0.0.0.0/0**
``` 
{
   "Return": true,
   "SecurityGroupRules": [
       {
           "SecurityGroupRuleId": "sgr-0658b6287c8d5b7d2",
           "GroupId": "sg-05cff46203964e587",
           "GroupOwnerId": "**************",
           "IsEgress": false,
           "IpProtocol": "tcp",
           "FromPort": 443,
           "ToPort": 443,
           "CidrIpv4": "0.0.0.0/0"
       }
   ]
}
```

## Put Application Load Balancer (ALB) as a proxy to your ASG


> aws elb create-load-balancer --load-balancer-name my-lb --listeners "Protocol=HTTP, LoadBalancerPort=80, InstanceProtocol=HTTP, InstancePort=80" --subnets subnet-0ba68314d8a034b7b --security-groups sg-04061a7e2e3446baf
