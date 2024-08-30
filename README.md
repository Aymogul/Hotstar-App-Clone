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