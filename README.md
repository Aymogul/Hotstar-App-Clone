# Hotstar App Deployment Using Kubernetes with CI/CD Pipeline

![dcn-zaacs](./images/project-diagram.PNG)

This project demonstrates the deployment of the Hotstar application on a Kubernetes cluster using a CI/CD pipeline orchestrated by Jenkins. The pipeline is designed to automate the entire process of building, testing, securing, and deploying the Hotstar app, ensuring a robust and reliable deployment workflow.

GitHub:
```sh
git clone https://github.com/Aymogul/Hotstar-App-Clone.git
```
## Prerequisite
- AWS account setup
- Basic knowledge of AWS services
- Understanding of DevSecOps principles
- Familiarity with Docker, Jenkins, Java, SonarQube, AWS - CLI 
- Kubectl
- Terraform and Docker Scout

## Project Highlights:
- Automated Build and Deployment: The pipeline automatically builds the Docker image for the Hotstar app, tags it, and pushes it to Docker Hub.

- Security and Vulnerability Scanning: Docker Scout is integrated to perform security checks on Docker images to identify vulnerabilities and provide recommendations for improvement.

- Code Quality Assurance: SonarQube is used to perform thorough code analysis, ensuring only high-quality code is deployed.

- Scalable and Resilient Deployment: Kubernetes is used to deploy the application, providing high availability, scalability, and self-healing capabilities.

## Overview
This project showcases a modern CI/CD workflow that combines the power of Jenkins, Docker, Docker Scout, SonarQube, and Kubernetes to deliver a secure, scalable, and efficient deployment of the Hotstar app. It is ideal for teams looking to implement robust DevOps practices and automate their deployment pipelines.

## Step-by-Step Deployment Process

### Step 1: Setting up AWS EC2 Instance
- Creating an EC2 instance with Ubuntu AMI, t2.large, and 30 GB storage
- Assigning an IAM role with Admin access for learning purposes

### Step 2: Installation of Required Tools on the Instance
Writing a script to automate the installation of:
- Docker
- Jenkins
- Java
- SonarQube container
- AWS CLI
- Kubectl
- Terraform

### Step 3: Jenkins Job Configuration
Creating Jenkins jobs for:
- Creating an EKS cluster
- Deploying the Hotstar clone application
- Configuring the Jenkins job stages:
- Sending files to SonarQube for static code analysis
- Running npm install
- Implementing OWASP for security checks
- Installing and running Docker Scout for container security
- Scanning files and Docker images with Docker Scout
- Building and pushing Docker images
- Deploying the application to the EKS cluster

### Step 4: Clean-Up Process
- Removing the EKS cluster
- Deleting the IAM role
- Terminating the Ubuntu instance

### STEP 1A: Setting up AWS EC2 Instance and IAM Role
1. Sign in to the AWS Management Console: Access the AWS Management Console using your credentials
2. Navigate to the EC2 Dashboard: Click on the “Services” menu at the top of the page and select “EC2” under the “Compute” section. This will take you to the EC2 Dashboard.
3. Launch Instance: Click on the “Instances” link on the left sidebar and then click the “Launch Instance” button.
4. Choose an Amazon Machine Image (AMI): In the “Step 1: Choose an Amazon Machine Image (AMI)” section:
- Select “AWS Marketplace” from the left-hand sidebar.
- Search for “Ubuntu” in the search bar and choose the desired Ubuntu AMI (e.g., Ubuntu Server 22.04 LTS).
- Click on “Select” to proceed.
5. Choose an Instance Type: In the “Step 2: Choose an Instance Type” section:
- Scroll through the instance types and select “t2.large” from the list.
- Click on “Next: Configure Instance Details” at the bottom.
6. Configure Instance Details: In the “Step 3: Configure Instance Details” section, you can leave most settings as default for now. However, you can configure settings like the network, subnet, IAM role, etc., according to your requirements.
- Once done, click on “Next: Add Storage.”
7. Add Storage: In the “Step 4: Add Storage” section:
- You can set the size of the root volume (usually /dev/sda1) to 30 GB by specifying the desired size in the “Size (GiB)” field.
- Customize other storage settings if needed.
- Click on “Next: Add Tags” when finished.
8. Add Tags (Optional): In the “Step 5: Add Tags” section, you can add tags to your instance for better identification and management. This step is optional but recommended for organizational purposes.
- Click on “Next: Configure Security Group” when done.
9. Configure Security Group: In the “Step 6: Configure Security Group” section:
- Create a new security group or select an existing one.
- Ensure that at least SSH (port 22) is open for inbound traffic to allow remote access.
- You might also want to open other ports as needed for your application’s requirements.
- Click on “Review and Launch” when finished.
10. Review and Launch: Review the configuration details of your instance. If everything looks good:
- Click on “Launch” to proceed.
- A pop-up will prompt you to select or create a key pair. Choose an existing key pair or create a new one.
- Finally, click on “Launch Instances.”
11. Accessing the Instance: Once the instance is launched, you can connect to it using SSH. Use the private key associated with the selected key pair to connect to the instance’s public IP or DNS address.

### STEP 1B: IAM ROLE
Search for IAM in the search bar of AWS and click on roles.
![alt image](/images/role-a.PNG)

Click on Create Role
![text](/images/role-b.PNG)

Select entity type as AWS service

Use case as EC2 and click on Next.
![text](/images/role-c.PNG)

For permission policy select Administrator Access (Just for learning purpose), click Next.

![text](/images/role-d.PNG)
Provide a Name for Role and click on Create role.

![text](/images/iam-b.PNG)
Role is created

Now Attach this role to Ec2 instance that we created earlier, so we can provision cluster from that instance.

Go to EC2 Dashboard and select the instance.

Click on Actions –> Security –> Modify IAM role.
![text](/images/iam-a.PNG)

Select the role you created earlier
![text](/images/iam-b.PNG)

### Step 2: Installation of Required Tools on the Instance
Scripts to install Required tools

```sh
sudo su
nano script1.sh
```
Cretion of installation script for;Java, Jenkins, Docker

```sh
#!/bin/bash
sudo apt update -y
wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc
echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
sudo apt update -y
sudo apt install temurin-17-jdk -y
/usr/bin/java --version
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y
sudo apt-get install jenkins -y
sudo systemctl start jenkins
#install docker
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg -y
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo usermod -aG docker ubuntu
newgrp docker
```
Add this permission before running the script

```sh
sudo chmod 777 script1.sh
sh script1.sh
```
script 2 for the installation of Terraform, Kubectl, Aws cli

```sh
nano script2.sh
```
Add the following code
```sh
#!/bin/bash
#install terraform
sudo apt install wget -y
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
#install Kubectl on Jenkins
sudo apt update
sudo apt install curl -y
curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
#install Aws cli
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt-get install unzip -y
unzip awscliv2.zip
sudo ./aws/install
```
Add this permission before running the script

```sh
sudo chmod 777 script1.sh
sh script1.sh
```

Start a sonarqube container with the following command:

```sh
sudo chmod 777 /var/run/docker.sock
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
```
Create an EC2 on the AWS console
![text](/images/ec2.PNG)

Copy the ec2 public address and map it to the jenkins server on port 8080
```sh
<ec2 ip address:8080> # login to jenkins page
```
![text](/images/jenkins-login-page.PNG)

Connect your Instance to Putty or Mobaxtreme and provide the below command for the Administrator password
```sh
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
![test](/images/jen-pasword.PNG)

Install suggested plugins
![text](/images/Cus-jenkins.PNG)

Jenkins will now get installed and install all the libraries.

Create an admin user
![text](/images/jenjins-admin-user.PNG)

Click on save and continue.

Jenkins Dashboard
![text](/images/jen-dashboard.PNG)

Now Copy the public IP again and paste it into a new tab in the browser with 9000
```sh
<ec2-ip:9000>  #runs sonar container
```
![alt](/images/sonar-login.PNG)

Enter username and password, click on login and change password
```sh
username admin
password admin
```
![alt](/images/sonar-update%20user.PNG)
Update New password.
This is Sonar Dashboard.
![text](/images/soanr-dashboard.PNG)

check for the version of things installed 
```sh
docker --version
aws --version
terraform --version
kubectl version
```
