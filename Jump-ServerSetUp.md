
# Phase 1: Baston Host Setup to Access EKS Cluster using kubectl

## Prerequisites
- Basic knowledge of Docker, and AWS services.
- An AWS account with necessary permissions.

## Steps 

### Step 1: EC2 Setup
- Launch an Ubuntu (t2.micro) instance in your favourite region (eg. region `us-west-2`).
- SSH into the instance from your local machine.

### Step 2: Clone the Code:
- Update all the packages and then clone the code.
- Clone your application's code repository onto the EC2 instance:

``` shell
git clone https://github.com/Vishvanath-Patil/Three-Tier-Application-Deployment-Project.git
```

### Step 3: IAM Configuration
- Create a user `eks-admin` with `AdministratorAccess`.
- Generate Security Credentials: Access Key and Secret Access Key.
  
### Step 4: Install AWS CLI v2
``` shell
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install -i /usr/local/aws-cli -b /usr/local/bin --update
aws configure
```

### Step 5: Install Docker
``` shell
sudo apt-get update
sudo apt install docker.io
docker ps
sudo chown $USER /var/run/docker.sock
sudo usermod -aG docker $USER
sudo systemctl status docker
#If the Docker daemon is not running, start it using
sudo systemctl start docker
#Check Docker Socket Permissions:
#Ensure that the permissions on the Docker socket file (/var/run/docker.sock) allow the user or the docker group to access it. You can check the permissions with:
ls -l /var/run/docker.sock
#It should have read and write permissions for the user or be owned by the docker group.
sudo systemctl restart docker #Restart docker

```

### Step 6: Install kubectl
``` shell
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
```

### Step 7: Install eksctl
``` shell
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```
