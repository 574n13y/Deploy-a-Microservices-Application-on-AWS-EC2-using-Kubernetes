# Deploy-a-Microservices-Application-on-AWS-EC2-using-Kubernetes
How to Deploy a Microservices Application on AWS EC2 using Kubernetes: A Step-by-Step 

 - Creating Two t2.medium Instances - To create two t2.medium instances on AWS EC2, follow these steps
   1. Log in to your AWS console and go to the EC2 dashboard.
   2. Click on the Launch Instance button.
   3. Choose Ubuntu Server 22.04 LTS (HVM) as the AMI.
   4. Choose t2.medium as the instance type.
   5. Click on Next until you reach the Configure Security Group page.
   6. Create a new security group with the following rules:
       a. Allow SSH from anywhere (port 22)
      b. Allow TCP from anywhere (port 80)
      c. Allow TCP from anywhere (port 5000)
      d. Allow TCP from anywhere (port 6443)
      e. Allow all traffic from within the security group (port range 0–65535)
   7. Click on the Review and Launch button.
   8. Create a new key pair or use an existing one and download it.
   9. Click on the Launch Instances button
     
 - Configuring Security Groups and SSH Keys
   1. Go to the Instances page on the EC2 dashboard and select one of your instances.
   2. Click on Actions > Networking > Change Security Groups.
   3. Select the security group that you created in the previous step and click on the Assign Security Groups button.
   4. Repeat the same steps for the other instance.
   5. Go to your terminal and change the permissions of your key pair file by running:
   6. SSH into one of your instances by running:
   7. Repeat the same steps for the other instance.
 - 
