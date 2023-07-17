# Project-cloud

---

Table of Contents

1.  [App deployment](#deployment)
2.  [Quizz](#quizz)
3.  [IAM policies](#iam)
4.  [Quicksight dashboard](#dashboard)

---
<div id='deployment'/>
  
## App deployment

The purpose of this project is to deploy an application on AWS to make sure that Social Research Organization has a secure and robust website. You can find more information on our objectives on the subject : https://github.com/pascalito007/efrei-cloud-bigdata/tree/master/capstone-project  

The first step was to define our infrastructure. We chose to work on the following one: 
![image](img-deployment/architecture.png)

Our PHP application will be in the EC2 Instance in the public subnet. With the VPC and its internet gateway. The users, here represented by the PC, will be able to access our application.

To keep our data safe, we can use Amazon RDS with MariaDB, with the Systems Manager Parameter Store, all the connection information will be stored and accessible by the application.

Please note that we created our own network stack while doing this project. 
<br><br>
Let’s apply the previous schema by creating a new VPC.
<br><br>
![image](img-deployment/deployment1.png)
<br><br>
Then we will create a new public subnet
<br><br>
![image](img-deployment/deployment2.png)
<br><br>
We then created a new Internet Gateway
<br><br>
![image](img-deployment/deployment3.png)
<br><br>
Next, we added a route to our VPC route table. This route is to connect our VPC to our internet gateway.
<br><br>
![image](img-deployment/deployment4.png)
<br><br>
The next step is to create a new EC2 instance based on the following AMI: Cloud9AmazonLinux2-2023-06-22T17-21. This instance is linked to our public subnet.
<br><br>
![image](img-deployment/deployment5.png)
<br><br>
We can see that our website is running (after we added a new inbound rule for http and did all the installations required). We connected a terminal using SSH connection. 
<br><br>
![image](img-deployment/deployment6.png)
<br><br>
![image](img-deployment/deployment7.png)
<br><br>
![image](img-deployment/deployment8.png)
<br><br>
Now that we have our first subnet ready to run our instance, we had to create another one for our database. We specifically chose to create the two subnets in two different regions (to create the RDS database properly)
<br><br>
![image](img-deployment/deployment9.png)
<br><br>
We then created a MariaDB database container on RDS. We created a new security group.
<br><br>
![image](img-deployment/deployment10.png)
<br><br>
![image](img-deployment/deployment11.png)
<br><br>
The next step was to connect to this database. To do so, we checked the databases, we saw that we had to create a new one. So, we created capstonedb.
<br><br>
![image](img-deployment/deployment12.png)
<br><br>
![image](img-deployment/deployment13.png)
<br><br>
We then proceed to use this DB and to run the script.
<br><br>
![image](img-deployment/deployment14.png)
<br><br>
Once it was done, we knew that we needed to allow the app to access the connection information. We did that part using the AWS Systems Manager. We created the 4 important endpoints for the app.
<br><br>
![image](img-deployment/deployment15.png)
<br><br>
And it worked! We can see that the queries are working on our application.
<br><br>
![image](img-deployment/deployment16.png)
<br><br>
![image](img-deployment/deployment17.png)

<div id='quizz'/>
  
## Quizz

Network:

1: Answer 3 <br>
2: Answer 3 <br>
3: Answers 1, 3 and 6<br>
4: Answer 4 

IAM: 

1: Answer 3 <br>
2: Answer 1<br>
3: Answers 3 and 4<br>
4: Answer 1<br>
5: Answer 1<br>
6: Answer 3<br>
7: Answer 1<br>


<div id='quizz'/>
  
## IAM policies

Question1: what actions are allowed for EC2 instances and S3 objects based on this policy? What specific resources are included?
<br><br>
![image](img-policies/policy1.png)
<br><br>
EC2 instances
•	“ec2:RunInstances”: allows the user to launch new EC2 instances.
•	“ec2:TerminateInstances”: allows the user to terminate EC2 instances.
•	“arn:aws:ec2:us-east-1:123456789012:instance/*: allows all actions on all EC2 instances in the specified region (us-east-1) for the account ID 123456789012.

S3 instances
•	“s3:GetObject”: allows the user to retrieve S3 objects
•	“s3:PutObject”: allows the user to upload S3 objects
•	“arn:aws:s3:::example-bucket/*”: allows actions on all objects within the example-bucket S3 bucket. 

Question2: under what condition does this policy allows access to VPC-related information? Which AWS region?
<br><br>
![image](img-policies/test.png)
<br><br>
•	“aws:RequestRegion”: “us-west-2” : allows access to VPC-related information only if the requested AWS region is set to “us-west-2”.

Question3: what actions are allowed on the “example-bucket” and its objects based on this policy? What specific prefixes are specified in the condition?
<br><br>
![image](img-policies/policy3.png)
<br><br>
Actions allowed on “example-bucket”:
•	“s3:GetObject”: allows the user to retrieve objects from the “example-bucket”.
•	“s3:PutObject”: allows the user to upload objects into the “example-bucket”.
•	“s3:ListBucket”: allows the user to list objects within the “example-bucket”.

Specified prefixes in the condition:
•	“documents/*”: allows access to objects in the “bucket-example” that have the “document” prefix.
•	“images/*”: allows access to objects in the bucket-example” that have the “images” prefix.

Question4: what actions are allowed for IAM users based on this policy? How are the resources ARNs constructed?
<br><br>
![image](img-policies/policy4.png)
<br><br>
Allowed actions:
•	“iam:CreateUser”: allows the user to create IAM users.
•	“iam:DeleteUser”: allows the user to delete IAM users.

Resource ARNs:
•	Follows the following template “arn:aws:iam::123456789012:user/${aws:username}”
•	“${aws:username} is dynamically replaced by the username of the user currently connected. Ensures the users will only perform their allowed actions on their own IAM user resource.

Question5: 
•	Which AWS service does this policy grant you access to?
•	Does it allow you to create an IAM user, group, policy, or role?
•	Go to https://docs.aws.amazon.com/IAM/latest/UserGuide/ and in the left navigation expand Reference > Policy Reference > Actions, Resources, and Condition Keys. Choose Identity And Access Management. Scroll to the Actions Defined by Identity And Access Management list.��Name at least three specific actions that the iam:Get* action allows.
<br><br>
![image](img-policies/policy5.png)
<br><br>
•	This policy grant access to all actions that start with Get “iam:Get*” and all actions that start with List “iam:List*”.
•	It does not allow to create anything. For that we would need to add “iam:Create*” to the actions allowed.
•	“iam:Get*” allows
o	“Iam:GetGroup”: allows to retrieve a list of IAM users that are in the specified IAM group.
o	“iam:GetRole”: allows to retrieve information about the specified role (role’s path, GUID, ARN, role’s trust policy).
o	“iam:GetUser”: allows to retrieve information about the specified IAM user (user’s creation date, path, unique ID and ARN). 

Question6: 
<br><br>
![image](img-policies/policy6.png)
<br><br>
•	What actions does the policy allow?

The actions “ec2:RunInstances” and “ec2:StartInstances” are denied to the user this policy is assigned to. However all actions not lister in the actions will be allowed to the user. 

•	Say that the policy included an additional statement object, like this example:
<br><br>
![image](img-policies/policy7.png)
<br><br>
•	How would the policy restrict the access granted to you by this additional statement?
•	If the policy included both the statement on the left and the statement in question 2, could you terminate an m3.xlarge instance that existed in the account?

The “allow” statement takes precedence over the “deny” effect. Therefore, the user will have access to all actions on ec2 instances and the actions defined before will not be taken into account.
You would be able to terminate an instance as you would have all actions allowed on ec2 instances.

<div id='dashboard'/>
  
## Quicksight dashboard
