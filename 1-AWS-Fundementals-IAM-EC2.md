# 1-AWS-Fundementals-IAM-EC2

## AWS Regions & AZs
names can be us-east-1, eu-west2 ..
eg:- US-east-1 -> N Verginia

- each region has many availability zones usually 3, min -2, max is 6
for eg:-
ap-southeast-2(Region) => ap-southeast-2A(AavailabilityZone) + ap-southeast-2B(AavailabilityZone) + ap-southeast-2C(AavailabilityZone)


- each AZ is one or more discrete data centers with redundant power, networking and connectivity, they are separate from each other so that they are isolated from disasters.
AZ's are connected with high bandwidth, ultra-low latency networking.

- find more info here : https://aws.amazon.com/about-aws/global-infrastructure/

Created my user account to login
https://ven-chad.signin.aws.amazon.com/console

## EC2
It mainly consists of
- Renting virtual machines (EC2)
- Storing data on virtual drives (EBS)
- Distributing load across machines (ELB)
- Scaling the services using an auto-scaling group (ASG)

Knowing EC2 is fundamental to understand how the cloud works

step1 - choose an AMI(AMazon machine Image) eg:- Linux2
step2 - choose a type eg:- t2.micro
step3 - configure instance details eg:- 1
step4 - add storage
step5 - add tags
step6 - add security group
step7 - review and launch
it ask to create a key pair for first time if you dont have create a new keypair

## SSH 
- ssh into your EC2 instance using Linux/Mac
- ssh is one of the most imp func. It allows you to control a remote machine, all using the command line
- get your IPv4 public IP from the instance description section(Ec2 instance) x.y.z.a
open your mac terminal and make sure in the same path directory you have the keypair that you donwloaded
Now run below command
`$ ssh -i my-EC2-tutorial.pem ec2-user@18.216.31.80`
You will see a warning like below
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'my-EC2-tutorial.pem' are too open.

So do chmod that file
`$chmod 0400 my-EC2-tutorial.pem`
then run 
`$ ssh -i my-EC2-tutorial.pem ec2-user@x.y.z.a`
now you will enter into EC2 instance

```
[ec2-user@ip-172-31-32-92 ~]$ whoami
ec2-user
[ec2-user@ip-172-31-32-92 ~]$ ping google.com
[ec2-user@ip-172-31-32-92 ~]$ ctrl+c
[ec2-user@ip-172-31-32-92 ~]$ logout
```

### EC2 instance connect
- its another way to connect to the instance with browser
- In instances you would see `connect` button on the top, click on that -> you would see that it connects in the browser
- this wont connect if we remove inbound rule to port 22


### Intro to security groups
- security groups are the fundemental of network - allow, inbound, outbound rules
- filter ip/port with rules
- they can be attached to multiple instances
- locked down to a region/vpc combination
- does live "outside" the EC2 - if traffic is blocked the EC2 instance wont see it.
- its good to maintain one separate security group for SSH access
- if your app is not accessible(timeout), then its a security group issue
- if your app gives a "connection refused" error, then its an application error or its not launched.
- private IP - two diff private networks(two companies) can have the same IPs.
- machines connect to www using NAT+internet gateway (a proxy)
- only a specified range of IPs can be used as private IP

### Elastic IPs
fixed IP







