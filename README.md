# Enterprise-GitLab-CI-CD
Automate enterprise software delivery with GitLab CI/CD pipeline. Deploying a  Board Game Database Full-Stack Web App on AWS EC2. Streamline code push, tool installation, testing, Docker image build, Aqua Trivy scan, and Kubernetes deployment using YAML manifests.

# Project Architecture

# The Setup Environment(Technologies)
1. Gitlab
2. Maven
3. AWS
4. Aqua Trivy
5. Sonarqube
6. Docker
7. Kubernetes

# Flow of Execution
* Push code to GitLab
* Build Pipeline on Gitlab
* Install all the tools: Trivy, JDK, Docker, SonarQube, Maven
* Perform unit testing with MAVEN
* Perform dependency check using Aqua Trivy
* Perform SonarQube for quality check and code coverage
* Build the application to create an artifact with MAVEN
* Build the Docker image of the application using Docker
* Scan the Docker image using Aqua Trivy for any vulnerability
* Use YAML manifest files to deploy the application to a Kubernetes cluster

# Push code to GitLab
  Import `project` from github Repository on GitLab website:
  
  <img width="1280" alt="Screenshot 2024-07-18 at 15 21 11" src="https://github.com/user-attachments/assets/cfca175e-ff74-48dd-87b3-3e18b6db39ef">


  Import Git repository using URL(https), create project for example - Boardgame, as shown below:
  
  

  <img width="1280" alt="Screenshot 2024-07-18 at 16 01 56" src="https://github.com/user-attachments/assets/9350d5ac-42ab-42b2-8aa9-031c7d3940fb">

  
  Launch an EC2 Instance on AWS cloud platform:


   Create an EC2 instance on AWS and give it a name of your choice,i used "corporate-dev".Configure the security group with the following inbound rules as  shown below.Choose a T2.large instance type,ensure storage is above 15GB,launch the instance and copy the public IP address of the instance.
  
 
  
  <img width="1280" alt="Screenshot 2024-07-18 at 18 34 14" src="https://github.com/user-attachments/assets/42457003-6ee5-4308-bb50-9ec4dd099b3b">

 
  Connect to EC2 Instance through your preffered terminal,i used TABBY terminal to connect to the EC2 instance.After connecting, run sudo apt update to update   the available packages.This virtual machine will act as the GitLab Runner.Now navigate to Gitlab and Setup GitLab Runner by clicking the project settings,     then to the CI/CD settings.Access the Runners section under CI/CD settings.

  In GitLab Runner section, create a project with a tag called "news-devs" or any name of your choice.Tick the 'Run untagged jobs' option.Click 'Create   runner'.
  Select the platform (Linux, Mac, Windows, or containers). In this case, use Linux.Install GitLab Runner on your terminal by copying the provided code on the   page and as shown below.

  <img width="1280" alt="Screenshot 2024-07-18 at 18 47 06" src="https://github.com/user-attachments/assets/bdc0d050-55a4-4c09-b4da-a9018ae236e7">
  <img width="1280" alt="Screenshot 2024-07-18 at 18 50 40" src="https://github.com/user-attachments/assets/8707c66a-7103-4fea-a2ab-6c170ab6b0c3">


  Register the runner on the terminal you have choosen by pasting the first steps displayed on the gitlab page abov and following the prompts.You will be   asked to Provide the GitLab instance url,simply use "URL: https://gitlab.com".Name the runner (e.g., corporate-dev) or any of your choice.Choose "SHELL" as   the executor since it can run all commands in the terminal.A message will pop indicating "The runner was registered successfully".Verify Runner Registration
by Navigating back to GitLab.See a message stating "You have registered a new runner!"
Click on 'View runners' to see the newly created runner.

# Build Pipeline on Gitlab

  

  

  






