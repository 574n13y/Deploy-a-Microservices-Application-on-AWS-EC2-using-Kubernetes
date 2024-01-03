# Deploy-a-Microservices-Application-on-AWS-EC2-using-Kubernetes
How to Deploy a Microservices Application on AWS EC2 using Kubernetes: 

- **_Setting up AWS EC2 Instances: We created two `t2.medium` instances on AWS EC2, one for the master node and one for the worker node. We also configured the security groups and SSH keys for these instances._**
- **_Installing Kubernetes on AWS EC2 Instances: We installed Kubernetes on both instances using `kubeadm`, `kubelet`, and `kubectl`. We also initialized the master node and joined the worker node to the cluster._**
- **_Deploying the Microservices Application on Kubernetes: We deployed a microservices application that consists of a MongoDB database and a taskmaster service. The taskmaster service is a web app that allows us to create and manage tasks. The app is written in Node.js and uses MongoDB as the database. We deployed the persistent volume and persistent volume claim, the `MongoDB` database, the `ClusterIP` service for MongoDB, the `taskmaster` service, and the `NodePort` service for taskmaster._**
  
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
      ```
      sudo apt update
      sudo apt install apt-transport-https ca-certificates curl -y
      curl -fsSL "https://packages.cloud.google.com/apt/doc/apt-key.gpg" | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/kubernetes-archive-keyring.gpg
      echo 'deb https://packages.cloud.google.com/apt kubernetes-xenial main' | sudo tee /etc/apt/sources.list.d/kubernetes.list
      ```
   4. Install `kubeadm`, `kubelet`, and `kubectl` by running:
      ```
      sudo apt update
      sudo apt install kubeadm=1.20.0-00 kubectl=1.20.0-00 kubelet=1.20.0-00 -y
      ```
   5. Verify that `kubeadm`, `kubelet`, and `kubectl` are installed by running:
      ```
      kubeadm version
      kubelet --version
      kubectl version --client
      ```
   6. Repeat the same steps for the other instance.
      
 - Initializing the Master Node
 - To initialize the master node, follow these steps: 
   1. SSH into the instance that you want to use as the master node by running: `` ssh -i ~/.ssh/mykey.pem ubuntu@<master-node-ip> ``
   2. Replace `~/.ssh/mykey.pem` with the path to your key pair file and `<master-node-ip>` with the public IPv4 address of your master node instance
   3. Initialize the master node by running: `` sudo kubeadm init ``
   4. Copy the join command that is displayed at the end of the output. You will need this command later to join the worker node to the cluster.
   5. Configure your user account to use `kubectl` by running:
      ```
      mkdir -p $HOME/.kube
      sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
      sudo chown $(id -u):$(id -g) $HOME/.kube/config
      ```
   6. Install a pod network add-on by running: [kube-flannel.yml](https://github.com/574n13y/Deploy-a-Microservices-Application-on-AWS-EC2-using-Kubernetes/blob/main/kube-flannel.yml)
      ```
      kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
      ```
   7. Generate a token for worker nodes to join:
      ```
      sudo kubeadm token create --print-join-commandsudo kubeadm token create --print-join-command
      ```
   8. Verify that the master node is ready by running: `` kubectl get nodes ``
      
 - Joining the Worker Node to the Cluster
 - To join the worker node to the cluster, follow these steps:
   1. SSH into the instance that you want to use as the worker node by running: `` ssh -i ~/.ssh/mykey.pem ubuntu@<worker-node-ip> ``
   2. Replace `~/.ssh/mykey.pem` with the path to your key pair file and `<worker-node-ip>` with the public IPv4 address of your worker node instance.
   3. Run the following command `` sudo kubeadm reset pre-flight checks ``
   4. Join the worker node to the cluster by running the join command that you copied from the master node. The command should look something like this:
      ```
      sudo kubeadm join 172.31.29.92:6443 --token thclqt.0m4dzsh5m6haswf5 --discovery-token-ca-cert-hash sha256:51bc85440423635ec77009514443330cfd2ff6292491dd33833e37406b34a8ba

      ```
   5. Replace `<master-node-ip>` with the public IPv4 address of your master node instance, `<token>` with the token that was generated by kubeadm, and <hash> with the hash of the CA certificate.
   6. Verify that the worker node is ready by running: `` kubectl get nodes ``
  
  
 - Deploying the Microservices Application on Kubernetes
 - Cloning the GitHub Repository
   1. SSH into the master node by running: `` ssh -i ~/.ssh/mykey.pem ubuntu@<master-node-ip> ``
   2. Replace `~/.ssh/mykey.pem` with the path to your key pair file and `<master-node-ip>` with the public IPv4 address of your master node instance.
   3. Install git by running: `` sudo apt install git -y ``
   4. Clone the repository that contains the configuration files for our MongoDB database by running: `` https://github.com/574n13y/microservices-k8s.git ``


 - Deploying the Persistent Volume and Persistent Volume Claim
   1. Go to the mongodb-k8s directory by running: `` cd microservices-k8s/flask-api/k8s ``
   2. Apply the YAML file that defines the persistent volume by running: `` kubectl apply -f mongo-pv.yml ``
   3. Apply the YAML file that defines the persistent volume claim by running: `` kubectl apply -f mongo-pvc.yml ``
   4. Verify that the persistent volume and persistent volume claim are created and bound by running: `` kubectl get pv,pvc ``
      
 - Deploying the MongoDB Database
   1. Apply the YAML file that defines the deployment by running: `` kubectl apply -f mongo.yml ``
   2. Verify that the deployment is created and running by running: `` kubectl get deployments ``
   3. Verify that the pod is running by running: `` kubectl get pods ``
      
 - Deploying the ClusterIP Service for MongoDB
   1. Apply the YAML file that defines the service by running: `` kubectl apply -f mongo-svc.yml ``
   2. Verify that the service is created by running: `` kubectl get services ``
      
 - Deploying the Taskmaster Service
   1. Apply the YAML file that defines the deployment by running: `` kubectl apply -f taskmaster.yml ``
   2. Verify that the deployment is created and running by running: `` kubectl get deployments ``
   3. Verify that the pods are running by running: `` kubectl get pods ``
      
 - Deploying the NodePort Service for Taskmaster
   1. Apply the YAML file that defines the service by running:
      ```
      kubectl apply -f taskmaster-svc.yml
      ```
   2. Verify that the service is created by running:
      ```
      kubectl get services
      ```
   3. Access the taskmaster web app from your browser by going to http://<node-ip>:30007/. You can use any node’s IP address, either the master or the worker.
   
   




