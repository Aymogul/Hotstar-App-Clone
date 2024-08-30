# Hotstar App Deployment Using Kubernetes with CI/CD Pipeline

![image](./images/project-diagram.PNG)

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