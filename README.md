# AWS CLI Command Reference

This repository contains a collection of AWS CLI commands for managing various AWS services including EC2, EBS, S3, VPC, IAM, Lambda, RDS, and SNS. Each section is organized by service type for easy reference.

## 🎥 Video link: [70 AWS CLI Commands](https://www.youtube.com/watch?v=fcH3poS5KME)



## EC2

Manage Amazon EC2 instances efficiently with these commands. 


1. **Retrieve an EC2 Instance ID**
```bash  

aws ec2 describe-instances --query "Reservations[].Instances[].InstanceId" --output text
```

2. **Stop an EC2 Instance** 

```bash
aws ec2 start-instances --instance-ids <instance_id>
```


3. **Start an EC2 Instance**  

```bash
aws ec2 start-instances --instance-ids <instance_id>
```


4. **Terminate an EC2 Instance**  

```bash
aws ec2 terminate-instances --instance-ids <instance_id>
```


5. **Launch an EC2 Instance**  
```bash
aws ec2 run-instances --image-id <ami-id> --count 1 --instance-type <instance-type> --key-name <key-pair-name> --security-group-ids <security-group-name> --subnet-id <subnet-id>
```


6. **Modify an Instance Type**  
```bash
aws ec2 modify-instance-attribute --instance-id <instance_id> --instance-type "{"Value": "t2.large"}"
```


7. **Describe EC2 Instances with Tags**  
```bash
aws ec2 describe-instances --filters "Name=tag:Name,Values=dev-instance"
```


8. **List All EC2 Instances in Running State**  
```bash
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" --query "Reservations[].Instances[].[InstanceId,InstanceType,State.Name,PublicIpAddress]" --output table
```


9. **Get Console Output for an Instance**  
```bash
aws ec2 get-console-output --instance-id <instance_id>
```


## EBS

Manage Amazon EBS volumes effectively with these commands

10. **Create an EBS Volume**  
 ```
 aws ec2 create-volume --size 10 --volume-type gp3 --availability-zone us-east-1a
 ```

11. **Attach an EBS Volume to an Instance**  
 ```
 aws ec2 attach-volume --volume-id <volume_id> --instance-id <instance_id> --device /dev/sdf
 ```

12. **Detach an EBS Volume**  
 ```
 aws ec2 detach-volume --volume-id <volume_id>
 ```

13. **Delete an EBS Volume**  
 ```
 aws ec2 delete-volume --volume-id <volume_id>
 ```

14. **Describe an EBS Volume**  
 ```
 aws ec2 describe-volumes --volume-ids <volume_id>
 ```

15. **List EBS Volumes in Available State**  
 ```
 aws ec2 describe-volumes --query "Volumes[?State!='in-use'].{ID:VolumeId, Size:Size, State:State}" --output table
 ```

16. **Modify an EBS Volume**  
 ```
 aws ec2 modify-volume --volume-id <volume_id> --size 20
 ```

17. **Describe EBS Volume by Tags**  
 ```
 aws ec2 describe-volumes --filters "Name=tag:Name,Values=MyVolume"
 ```

## Snapshots

Handle AWS EBS snapshots efficiently with these commands. 

18. **Create a Snapshot**  
 ```
 aws ec2 create-snapshot --volume-id <volume_id> --description "My snapshot"
 ```

19. **List All Snapshots**  
 ```
 aws ec2 describe-snapshots --owner-ids 123456789012
 ```

20. **Delete a Specific Snapshot**  
 ```
 aws ec2 delete-snapshot --snapshot-id <snapshot_id>
 ```

21. **Copy a Snapshot from One Region to Another**  
 ```
 aws ec2 copy-snapshot --source-region us-east-1 --source-snapshot-id <snapshot_id> --destination-region us-west-2 --description "Snapshot copy to us-west-2"
 ```

22. **Modify Snapshot Permission**  
 ```
 aws ec2 modify-snapshot-attribute --snapshot-id <snapshot_id> --attribute createVolumePermission --operation-type add --user-ids 123456789012
 ```

23. **List Snapshot Based on Specific Tags**  
 ```
 aws ec2 describe-snapshots --filters "Name=tag:Name,Values=MyProject"
 ```

## S3 Commands

Manage Amazon S3 buckets and objects efficiently with these commands:

24. **Create an S3 Bucket**  
    ```bash
    aws s3 mb s3://mybucket
    ```
25. **List all S3 Buckets**  
    ```bash
    aws s3 ls
    ```
26. **Upload a File to an S3 Bucket**  
    ```bash
    aws s3 cp localfile.txt s3://mybucket/
    ```
27. **Delete a Bucket and All Its Contents**  
    ```bash
    aws s3 rb s3://mybucket --force
    ```
28. **List Objects in an S3 Bucket**  
    ```bash
    aws s3 ls s3://mybucket --recursive
    ```
29. **Copy an Object Between S3 Buckets**  
    ```bash
    aws s3 cp s3://mybucket1/myobject.txt s3://mybucket2/myobject.txt
    ```
30. **Delete an Object in an S3 Bucket**  
    ```bash
    aws s3 rm s3://mybucket/myobject.txt
    ```
31. **Enable Versioning in an S3 Bucket**  
    ```bash
    aws s3api put-bucket-versioning --bucket mybucket --versioning-configuration Status=Enabled
    ```

## VPC Commands

Efficiently manage your AWS Virtual Private Cloud (VPC) environments with these commands:

32. **Create a VPC**  
    ```bash
    aws ec2 create-vpc --cidr-block 10.0.0.0/16
    ```
33. **Get the List of VPC IDs**  
    ```bash
    aws ec2 describe-vpcs --query 'Vpcs[*].VpcId' --output text
    ```
34. **Delete a Specific VPC**  
    ```bash
    aws ec2 describe-vpcs --query 'Vpcs[*].VpcId' --output text
    ```
35. **Create a Subnet**  
    ```bash
    aws ec2 create-subnet --vpc-id vpc-1234abcd --cidr-block 10.0.1.0/24
    ```
36. **Create an Internet Gateway and Attach to VPC**  
    ```bash
    aws ec2 create-internet-gateway
    aws ec2 attach-internet-gateway --vpc-id vpc-1234abcd --internet-gateway-id igw-1234abcd
    ```
37. **Create a Route Table and Associate It with Subnet**  
    ```bash
    aws ec2 create-route-table --vpc-id vpc-1234abcd
    aws ec2 associate-route-table --route-table-id rtb-1234abcd --subnet-id subnet-5678efgh
    ```
38. **Modify VPC Attribute and Enable DNS Hostname**  
    ```bash
    aws ec2 modify-vpc-attribute --vpc-id vpc-1234abcd --enable-dns-hostnames "{\"Value\":true}"
    ```
39. **Create a Security Group in a VPC**  
    ```bash
    aws ec2 create-security-group --group-name mySecurityGroup --description "My security group" --vpc-id vpc-1234abcd
    ```
40. **Add a Rule in Security Group**  
    ```bash
    aws ec2 authorize-security-group-ingress --group-id sg-1234abcd --protocol tcp --port 22 --cidr 0.0.0.0/0
    ```
41. **Create a NAT Gateway**  
    ```bash
    aws ec2 create-nat-gateway --subnet-id subnet-1234abcd --allocation-id eip-abcd1234
    ```
42. **Change the Security Group of an EC2 Instance**  
    ```bash
    aws ec2 modify-instance-attribute --instance-id <instance_id> --groups sg-98765432 sg-87654321
    ```

## IAM Commands

Simplify identity and access management in AWS with these IAM operations:

43. **Create an IAM User**  
    ```bash
    aws iam create-user --user-name myUser
    ```
44. **List IAM Users**  
    ```bash
    aws iam list-users --query 'Users[*].UserName' --output text
    ```
45. **Attach a Policy to an IAM User**  
    ```bash
    aws iam attach-user-policy --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess --user-name myUser
    ```
46. **Delete an IAM User**  
    ```bash
    aws iam delete-user --user-name myUser
    ```
47. **Create an Access Key and Secret Key for an IAM User**  
    ```bash
    aws iam create-access-key --user-name myUser
    ```
48. **Deactivate User Key**  
    ```bash
    aws iam update-access-key --access-key-id AKIAIOSFODNN7EXAMPLE --user-name myUser --status Inactive
    ```
## Lambda Commands

Efficiently manage your AWS Lambda functions with these straightforward commands:

49. **Create a Lambda Function**  
    ```bash
    aws lambda create-function --function-name myFunction --runtime nodejs12.x --role arn:aws:iam::123456789012:role/lambda-role --handler index.handler --zip-file fileb://function.zip
    ```
50. **List Lambda Functions**  
    ```bash
    aws lambda list-functions
    ```
51. **Invoke a Lambda Function**  
    ```bash
    aws lambda invoke --function-name myFunction --payload '{"key": "value"}' response.json
    ```
52. **Delete a Lambda Function**  
    ```bash
    aws lambda delete-function --function-name myFunction
    ```
53. **Update Lambda Function**  
    ```bash
    aws lambda update-function-code --function-name myFunction --zip-file fileb://function.zip
    ```

## RDS Commands

Efficiently manage your Amazon RDS instances with these commands:

54. **Create a Database Instance**  
    ```bash
    aws rds create-db-instance --db-instance-identifier mydbinstance --allocated-storage 20 --db-instance-class db.m1.small --engine mysql --master-username masteraws --master-user-password masterpassword
    ```
55. **List All RDS Instances**  
    ```bash
    aws rds describe-db-instances
    ```
56. **Delete a Database Instance (Skip Final Snapshot)**  
    ```bash
    aws rds delete-db-instance --db-instance-identifier mydbinstance --skip-final-snapshot
    ```
57. **Modify DB Instance**  
    ```bash
    aws rds modify-db-instance --db-instance-identifier mydbinstance --db-instance-class db.m4.large --apply-immediately
    ```
58. **Take a DB Snapshot**  
    ```bash
    aws rds create-db-snapshot --db-instance-identifier mydbinstance --db-snapshot-identifier mydbsnapshot
    ```
59. **Restore DB Snapshot**  
    ```bash
    aws rds restore-db-instance-from-db-snapshot --db-instance-identifier newdbinstance --db-snapshot-identifier mydbsnapshot
    ```
60. **Modify DB Instance Retention Policy**  
    ```bash
    aws rds modify-db-instance --db-instance-identifier mydbinstance --backup-retention-period 7 --apply-immediately
    ```
61. **Promote a Read Replica to Standalone Instance**  
    ```bash
    aws rds promote-read-replica --db-instance-identifier myreadreplica
    ```

## SNS Commands

Manage Amazon Simple Notification Service (SNS) effectively with these commands:

62. **Create a New SNS Topic**  
    ```bash
    aws sns create-topic --name myTopic
    ```
63. **Subscribe an Email Address to SNS Topic**  
    ```bash
    aws sns subscribe --topic-arn arn:aws:sns:us-west-2:123456789012:myTopic --protocol email --notification-endpoint example@example.com
    ```
64. **Publish a Message to Specific Topic**  
    ```bash
    aws sns publish --topic-arn arn:aws:sns:us-west-2:123456789012:myTopic --message "Hello world"
    ```
65. **Delete a SNS Topic**  
    ```bash
    aws sns delete-topic --topic-arn arn:aws:sns:us-west-2:123456789012:myTopic
    ```

## CloudWatch Commands

Effectively monitor and manage your AWS resources with these CloudWatch commands:

66. **Create a CloudWatch Alarm**  
    ```bash
    aws cloudwatch put-metric-alarm --alarm-name HighCPUUtilization --metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 300 --threshold 80 --comparison-operator GreaterThanOrEqualToThreshold --dimensions Name=InstanceId,Value=i-1234567890abcdef0 --evaluation-periods 2 --alarm-actions arn:aws:sns:us-west-2:123456789012:myTopic
    ```
67. **Delete a CloudWatch Alarm**  
    ```bash
    aws cloudwatch delete-alarms --alarm-names HighCPUUtilization
    ```
68. **Get Data About Specific Metric in a Given Time Frame**  
    ```bash
    aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --start-time 2021-01-01T00:00:00Z --end-time 2021-01-02T00:00:00Z --period 3600 --statistics Average --dimensions Name=InstanceId,Value=i-0123456789abcdef0
    ```
69. **Describe Alarm History of a Specific Alarm**  
    ```bash
    aws cloudwatch describe-alarm-history --alarm-name HighCPUUtilization
    ```
70. **Manually Change the State of an Alarm for Testing Purposes**  
    ```bash
    aws cloudwatch set-alarm-state --alarm-name "MyAlarm" --state-reason "Manual trigger for testing" --state-value ALARM
    ```
