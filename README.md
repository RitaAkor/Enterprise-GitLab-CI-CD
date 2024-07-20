# Enterprise-GitLab-CI-CD

Automate enterprise software delivery with GitLab CI/CD pipeline. Deploying a Board Game Database Full-Stack Web App on AWS EC2. Streamline code push, tool installation, testing, Docker image build, Aqua Trivy scan, and Kubernetes deployment using YAML manifests.

# Project Architecture
![350650311-0deb2aa0-e0a7-4ecb-9c78-cb989287516d](https://github.com/user-attachments/assets/f061ddca-e01e-47b4-9813-e3214bb61f12)

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
* Build the Docker image of the application using Docker and push
* Scan the Docker image using Aqua Trivy for any vulnerability
* Use YAML manifest files to deploy the application to a Kubernetes cluster

# Push code to GitLab
Import project from github Repository on GitLab website:
<img width="1280" alt="350013407-cfca175e-ff74-48dd-87b3-3e18b6db39ef" src="https://github.com/user-attachments/assets/a41d413e-1361-4bed-8e8a-1634cf6d0954">
Import Git repository using URL(https), create project for example - Boardgame, as shown below:

<img width="1280" alt="350019667-9350d5ac-42ab-42b2-8aa9-031c7d3940fb" src="https://github.com/user-attachments/assets/0ef47baa-cc2e-4d0c-b65c-30dff189f944">
Launch an EC2 Instance on AWS cloud platform:
Create an EC2 instance on AWS and give it a name of your choice,i used "corporate-dev".I used UBUNTU as my AMI.Configure the security group with the following inbound rules as shown below.Choose a T2.large instance type,ensure storage is above 15GB,launch the instance and copy the public IP address of the instance.

<img width="1280" alt="350072650-42457003-6ee5-4308-bb50-9ec4dd099b3b" src="https://github.com/user-attachments/assets/700145bb-22f8-4204-82d7-d0d08ab392b8">

Connect to EC2 Instance through your preffered terminal,i used TABBY terminal to connect to the EC2 instance.After connecting, run sudo apt update to update the available packages.This virtual machine will act as the GitLab Runner.Now navigate to Gitlab and Setup GitLab Runner by clicking the project settings, then to the CI/CD settings.Access the Runners section under CI/CD settings.
In GitLab Runner section, create a project with a tag called "news-devs" or any name of your choice.Tick the 'Run untagged jobs' option.Click 'Create runner'. Select the platform (Linux, Mac, Windows, or containers). In this case, use Linux.Install GitLab Runner on your terminal by copying the provided code on the page and as shown below.
<img width="1280" alt="350076191-bdc0d050-55a4-4c09-b4da-a9018ae236e7" src="https://github.com/user-attachments/assets/3b752436-de74-4ba9-985d-dfb47e56d45b">

<img width="1280" alt="350077086-8707c66a-7103-4fea-a2ab-6c170ab6b0c3" src="https://github.com/user-attachments/assets/5e52eb8c-9917-434b-80dd-cd0661d8ddb0">

Register the runner on the terminal you have choosen by pasting the first steps displayed on the gitlab page abov and following the prompts.You will be asked to Provide the GitLab instance url,simply use "URL: https://gitlab.com".Name the runner (e.g., corporate-dev) or any of your choice.Choose "SHELL" as the executor since it can run all commands in the terminal.A message will pop indicating "The runner was registered successfully".Verify Runner Registration by Navigating back to GitLab.See a message stating "You have registered a new runner!" Click on 'View runners' to see the newly created runner.

# Build Pipeline on Gitlab

Build the pipeline on GitLab by navigating to yout Boardgame project,go to the Build section and access the Pipeline Editor to start build the stages.

# Install all the tools: Trivy, JDK, Docker, Maven

This will be inputed on the build stage in gitlab as shown in the image below:
* Install Docker: sudo apt-get install -y docker.io Manage Docker Permissions:By default, only root users have permission to run Docker commands. You can either: Add the current user to the Docker group:sudo usermod -aG docker $USER or change the permissions on the Docker socket to allow other users to execute Docker commands by using:sudo chmod 666 /var/run/docker.sock
* Install kubectl using Snap: sudo snap install kubectl --classic
* Install Maven: sudo apt install -y maven
* Install Trivy: by visiting the Trivy documentation website and navigate to the installation section to get the commands to install Trivy. $ sudo vim /etc/yum.repos.d/trivy.repo [trivy] name=Trivy repository baseurl=https://aquasecurity.github.io/trivy-repo/rpm/releases/$releasever/$basearch/ gpgcheck=0 enabled=1 $ sudo yum -y update $ sudo yum -y install trivy Lastly include the 'TAG'(news-devs) section to define in the pipeline,that the stage is to run on the runneer configured.



