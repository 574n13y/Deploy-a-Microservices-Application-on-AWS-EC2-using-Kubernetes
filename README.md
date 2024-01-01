# Deploy-a-Microservices-Application-on-AWS-EC2-using-Kubernetes
How to Deploy a Microservices Application on AWS EC2 using Kubernetes: A Step-by-Step 

 ![image](https://github.com/574n13y/Deploy-a-Microservices-Application-on-AWS-EC2-using-Kubernetes/assets/35293085/fdd348ff-60e2-4f0c-82ec-e75fbcef69cf)

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
   5. Go to your terminal and change the permissions of your key pair file by running: ``chmod 400 ~/.ssh/mykey.pem ``
   6. Replace `~/.ssh/mykey.pem` with the path to your key pair file. This will make sure that only you can read and write to your key pair file.
   7. SSH into one of your instances by running: `` ssh -i ~/.ssh/mykey.pem ubuntu@<instance-ip> ``
   8. Replace ` ~/.ssh/mykey.pem ` with the path to your key pair file and `<instance-ip> ` with the public IPv4 address of your instance. This will establish a secure connection to your instance using your key pair.
   9. Repeat the same steps for the other instance.
       
 - Installing Kubernetes on AWS EC2 Instances
 - Installing Docker on Both Instances. To install Docker on both instances, follow these steps:
   1. SSH into one of your instances by running: `` ssh -i ~/.ssh/mykey.pem ubuntu@<instance-ip> ``
   2. Replace `~/.ssh/mykey.pem` with the path to your key pair file and `<instance-ip> ` with the public IPv4 address of your instance.
   3. Update the package index by running: `` sudo apt update ``
   4. Install Docker by running: `` sudo apt install docker.io -y ``
   5. Start and enable the Docker service by running:
      ```
      sudo systemctl start docker
      sudo systemctl enable docker
      ```
   6. Verify that Docker is installed and running by running: `` sudo docker version ``
   7. Add current user to the docker group `` sudo usermod -aG docker $USER ``
   8. Repeat the same steps for the other instance.
      
 - Installing `kubeadm`, `kubelet`, and `kubectl` on Both Instances. To install `kubeadm`, `kubelet`, and `kubectl` on both instances, follow these steps:
   1. SSH into one of your instances by running: `` ssh -i ~/.ssh/mykey.pem ubuntu@<instance-ip> ``
   2. Replace `~/.ssh/mykey.pem` with the path to your key pair file and `<instance-ip>` with the public IPv4 address of your instance.
   3. Add the Kubernetes apt repository by running:
   4. Install kubeadm, kubelet, and kubectl by running:
   5. Verify that kubeadm, kubelet, and kubectl are installed by running:
   6. Repeat the same steps for the other instance.
      
 - Initializing the Master Node
 - To initialize the master node, follow these steps:
   1. SSH into the instance that you want to use as the master node by running:
   2. Initialize the master node by running:
   3. Copy the join command that is displayed at the end of the output. You will need this command later to join the worker node to the cluster.
   4. Configure your user account to use kubectl by running:
   5. Install a pod network add-on by running:
   6. Generate a token for worker nodes to join:
   7. Verify that the master node is ready by running:
      
 - Joining the Worker Node to the Cluster


