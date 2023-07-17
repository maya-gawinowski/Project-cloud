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
![image](img-deployment/deployment1.png)
<br><br>
Then we will create a new public subnet
![image](img-deployment/deployment2.png)


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

<div id='dashboard'/>
  
## Quicksight dashboard
