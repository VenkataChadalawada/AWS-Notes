# classic solution architecture

- solution architecture mindset

## building whatistime.com
- scaling vertically (T2micro wont be enough --> M5 instance (with same elastic IP) we might have downtime for a bit
- scaling horizantally ( 3 M5 instances instead of 1) (Route53 with TTL 1 hour -> what if after 1 hour INSTANCE is gone! ? -> load balancer(ELB)
- `route53+ELB+M5 EC2 instances`
- adding and removing instances manually is pretty hard so we need Auto Scaling Group
- `route53 + ELB + AutoScalingGroup+ M5 EC2 instances`
- what if earth quauke come? -> multi AZ
- ` DNS + 3AZs for ELB+ ASG also has multi AZ with instances in those AGs
- minimum 2 AZ => Lets reserve capacity

publicIP vs privateIP and EC2 instances
ElasticIp vs Route53 vs Load Balancers
- Route 53 TTL, A records and Alias Records
- maintaining EC2 instances manually vs Auto Scaling Groups
- multi AZ to survive disasters
- ELB Health checks
- Security Group Rules
- Reservation of capacity for costing savings when possible
- we're considering 5 pillars fpr a well architected application: `costs, performance, reliability, security, operational Excellence`

### statefulweb App: Myclothes.com

About website
- myclothes.com allow people to buy clothes online,
- there's a shopping cart
- our website is having hundreads of users at the same time
- we need to scale, maintain horizontal scalability and keep our web application as stateless as possible
- Users should not lose their shopping cart
- Users should have their details (address etc) in a database

route53+multiAZ ELB+ASG(M5) EC2
- user looses session -> stickiness SessionAfinity? what if that instance goes down? -> web cookies? --> HTTP requests are heavier & security risk->cookies<4KB
- server sessions with Elastic Cache (or) Amazon DynamoDB
- storing data - RDS (address,name etc..) -> availability?& performance ->  replication [read & write replicas] (or) read from cache[check in cache then if not read DB]
- security groups 

- ELB sticky sessions
- webclients for storing cookies and making our web app stateless
- Elastic Cache
  - for storing sessions(alternative: DynamoDB
  - for caching data from RDS
  - Multi AZ
- RDS
  - for storing user data
  - read repolicas for scaling reads
  - multi AZ for disaster recovery
- Tight Security with security groups referencing each other



### Stateful web app: MyWordpress.com

- we are trying to create a fully scalable WordPress website
- we want that website to access and correctly display picture uploads
- our user data and the blog content should be stores in a MySQL database

Let's see how we can achieve this
- On top of Aurora MySQL MultiAZ Read Replicas for scalability
- for storing images EBS Volume  with each M5 instance in each AZ?
- scaling across multiple AZ's ? -> EFS (distributed application) creates ENI's (cost wise EFS is costlier than EBS - its a tradeoff) 

### Instantiating applications quickly
- when launching a full stack (EC2, EBS, RDS) , it can take time to:
  - install applications
  - insert initial or recovery data
  - configure everything
  - launch the application
- we can take advantage of the cloud to speed that up!

- EC2 instances:
  - Use a Golden AMI: install your applications, OS dependencies etc.. before hand and launch your EC2 instance from the Golden AMI (saves time)
  - Bootstrap using User Data: For dynamic configuration, use User Data scripts
  - Hybrid: mix Golden AMI and User Data (Elastic beanStalk)
- RDS Databases:
  - Restore from a snapshot: the database will have schemas and data ready!
- EBS Volumes:
  - Restore from snapshot: the disk will already be formatted and have data!

### Developer problems on AWS
- managing infrastructure
- Deploy code
- Configuring all the databased, load balancers, etc
- scaling concerns
- most web apps have the same acrhitecture (ALB + ASG)
- All the developers want is for their code to run!
- Possibly, consistently across different applications and environments

### Elastic Bean Stalk
- it is a developer centric view of deployiong an application in AWS
- it uses all the components we have seen before: EC2, ASG, ELB, RDS etc..
- But it is all in one view thats easy to make sense of!
- We still have full control over the configuration
- Beanstalk is free but you pay for the underlying instances
- Managed Service
  - Instance configurtaion/ OS is handled by beanstalk
  - deployment strategy is configurable but performed by EBS
- just the application code is the responsibility of the developer
- Three architecture models:
  - Single insatnce deployment: good for dev
  - LB+ASG: great for production or pre-production web applications
  - ASG only: great for non-web apps in production (workers, etc..)
- EBS has 3 components
  - application
  application version: each deployment gets assigned a version
  - Environment name (dev, test, prod..) free naming
  - you deploy application versions to environments and can promote application versions to the nest environment
  - Rollback feature to previous application version
  - Full control over lifecycle of environments
  - support got many platforms
  
  

  
  









