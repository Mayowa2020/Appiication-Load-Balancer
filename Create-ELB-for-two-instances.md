## Objectives

    1. Create Two EC2 Instances:

    - Learn how to launch two Amazon EC2 instances in a specified subnet.

    2. Set Up ALB:

    - Create an Application Load Balancer to distribute incoming traffic.

    3. Configure Target Group:

    - Set up a target group  to manage the registered EC2 instances

    4. Attach the created EC2 instances to the ALB.

    5. Test Load Balancing:

    - Verify that the ALB is distributing traffic evenly between the EC2 instances

## Project Tasks

Task 1: Create Two EC2 Instances:

Launch two Amazon EC2 instances in a specified subnet by following these steps:
Log in to your AWS Management Console.
Navigate to the EC2 dashboard.
Click on “Launch Instance” and choose the desired Amazon Machine Image (AMI).
Select the instance type, configure instance details, add storage, configure security groups, and review before launching.
Use the UserData Script below:

    #!/bin/bash
yum update -y
sudo yum install -y httpd httpd-tools mod_ssl
sudo systemctl enable httpd
sudo systemctl start httpd
sudo amazon-linux-extras enable php7.4
sudo yum clean metadata
sudo yum install php php-common php-pear -y
sudo yum install php-{cgi,curl,mbstring,gd,mysqlnd,gettext,json,xml,fpm,intl,zip,mysqli} -y
sudo rpm -Uvh <https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm>
sudo rpm --import <https://repo.mysql.com/RPM-GPG-KEY-mysql-2022>
sudo yum install mysql-community-server -y
sudo systemctl enable mysqld
sudo systemctl start mysqld
echo "fs-07d0a7f8f87044530.efs.eu-west-2.amazonaws.com:/ /var/www/html nfs4 nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2 0 0" >> /etc/fstab
mount -a
chown apache:apache -R /var/www/html
sudo service httpd restart

Repeat the process to launch the second EC2 instance.

Task 2: Set Up ALB:

Create an Application Load Balancer to distribute incoming traffic by:
Navigating to the EC2 dashboard and selecting “Load Balancers” under the “Load Balancing” section.
Click on “Create Load Balancer,” choose “Application Load Balancer,” configure settings like listeners and availability zones, set up security settings, configure routing, and review before creating the ALB.

Task 3: Configure Target Group:

Set up a target group to manage the registered EC2 instances by:
Going to the “Target Groups” section under the “Load Balancing” tab in the EC2 dashboard.
Click on “Create target group,” provide a name, protocol, port, VPC, health checks configuration, and register both EC2 instances with this target group.

Task 4: Attach EC2 Instances to ALB:

Attach the created EC2 instances to the ALB by:
Editing the listener rules of your ALB and adding a new rule that forwards traffic to your target group.
Ensure that both EC2 instances are healthy and registered with the target group.

Task 5:. Test Load Balancing:

Verify that the ALB is distributing traffic evenly between the EC2 instances by:
Accessing your ALB’s DNS name or IP address in a web browser multiple times.
Monitor which EC2 instance serves each request to confirm that load balancing is working as expected.
Importance of Load Balancing: Load balancing plays a crucial role in ensuring high availability and distributing workloads efficiently across multiple servers. It helps prevent overloading of any single server, improves fault tolerance, enhances performance, and ensures that applications remain accessible even if one server fails. By distributing incoming traffic evenly among multiple servers, load balancing contributes to scalability and resilience of applications hosted in cloud environments like AWS.

References:

[Setting up an Application Load Balancer with AWS EC2]
<https://medium.com/@sayalishewale12/setting-up-an-application-load-balancer-with-aws-ec2-39f7a74d89a>

[Create an Application Load Balancer (ALB) in AWS | AWS Elastic Load Balancing Tutorial for Beginners]
 <https://www.youtube.com/watch?v=ZGGpEwThhrM>

[Why can't I connect to a website that is hosted on my EC2 instance?]
<https://repost.aws/knowledge-center/ec2-instance-hosting-unresponsive-website>
