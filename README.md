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


   Create an EC2 instance on AWS and give it a name of your choice,i used "corporate-dev".I used UBUNTU as my AMI.Configure the security group with the following inbound rules as  shown below.Choose a T2.large instance type,ensure storage is above 15GB,launch the instance and copy the public IP address of the instance.
  
 
  
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
  Build the pipeline on GitLab by navigating to yout Boardgame project,go to the Build section and access the Pipeline Editor to start build the stages.

# Install all the tools: Trivy, JDK, Docker, Maven
   This will be inputed on the build stage in gitlab as shown in the image below:
   - Install Docker:               `sudo apt-get install -y docker.io` 
   Manage Docker Permissions:By default, only root users have permission to run Docker commands. You can either: Add the current user to the Docker group:`sudo usermod -aG docker $USER` or change the permissions on the Docker socket to allow other users to execute Docker commands by using:`sudo chmod 666 /var/run/docker.sock`
   - Install kubectl using Snap:   `sudo snap install kubectl --classic`
   - Install Maven:                `sudo apt install -y maven`
   - Install Trivy: by visiting the Trivy documentation website and navigate to the installation section to get the commands to install Trivy.
     $ sudo vim /etc/yum.repos.d/trivy.repo
     [trivy]
     name=Trivy repository
     baseurl=https://aquasecurity.github.io/trivy-repo/rpm/releases/$releasever/$basearch/
     gpgcheck=0
     enabled=1
     $ sudo yum -y update  
     $ sudo yum -y install trivy
  Lastly include the 'TAG'(news-devs) section to define in the pipeline,that the stage is to run on the runneer configured.
  
  <img width="1280" alt="Screenshot 2024-07-18 at 20 23 24" src="https://github.com/user-attachments/assets/79500fd5-8b80-43b0-a78d-78d5e2e41d9f">
  
# Perform unit testing with MAVEN

  To ensure that the code is functioning as expected we have to do automated tests.The "Unit Testing Stage" as shown the image below will catch any regressions or errors early in the development process before the code is deployed.
  Click on the commit change and wait for the results by viewing on the pipeline.
  This will install all the tools defined above.

  <img width="1280" alt="Screenshot 2024-07-18 at 20 28 43" src="https://github.com/user-attachments/assets/ca3e691f-677c-4442-980a-5833d42a2cb2">
 
  The completion image is below:


  <img width="1280" alt="Screenshot 2024-07-18 at 20 34 34" src="https://github.com/user-attachments/assets/dff9c7ec-bd8b-46b1-b9f5-fdad6955d4d1">

# Perform dependency check using Aqua Trivy

  The trivy_fs_scan stage in the CI/CD pipeline is designed to perform a file system security scan using Trivy, a vulnerability scanner. This section in the build stage,specifies that the trivy_fs_scan job belongs to the security stage of your pipeline. The security stage is dedicated to tasks related to scanning and analyzing security vulnerabilities.The script section contains the commands that will be executed during this stage.
  trivy fs --format table -o fs.html .: This command runs Trivy to scan the file system for vulnerabilities.
  trivy fs: Invokes Trivy to perform a file system scan.
--format table: Specifies the output format of the scan results. In this case, the results will be displayed in a table format.
-o fs.html: Directs Trivy to save the scan results to a file named fs.html in HTML format. This allows for easy viewing and sharing of the scan results.
.: Indicates the current directory as the target for the scan. Trivy will scan the entire file system starting from this directory

  <img width="1280" alt="Screenshot 2024-07-18 at 20 41 25" src="https://github.com/user-attachments/assets/252680a7-89e5-4fdd-a783-0b6e8d391552">

# Perform SonarQube for quality check and code coverage

  For sonarqube,the first thing to do since docker is already installed on the terminal through the pipeline is to run this command: `Docker run -d -p   9000:9000 nsonarqube:lts-community.
  The docker run: Starts a new Docker container.
  -d: Runs the container in the background (detached mode).
  -p 9000:9000: Maps port 9000 of the container to port 9000 on your host machine, making SonarQube accessible at http://localhost:9000.
  sonarqube:lts-community: Specifies the Docker image and tag for SonarQube’s LTS Community Edition.

 To access SonarQube, copy the IP address of the EC2 instance (named corporate-dev) from AWS, paste it into your browser, and append the port 9000. SonarQube will start, and you'll be able to configure it so that reports can be pushed to the SonarQube server through the pipeline.
  Configuration Steps;
    - Log In: Access SonarQube and log in with the username admin and password admin. Change the password to something secure.
    - Setup Integration with GitLab:
    - Go to the SonarQube settings.
    - Choose the GitLab option.
    - Enter a configuration name of your choice.
    - For GitLab API URL, use: https://gitlab.com/api/v4.
    - Generate a Personal Access Token:
    - Navigate to GitLab, then go to Settings > Access Tokens.
    - Click on Add New Token.
    - Provide a name for the token.
    - Select the Reporter role under Scopes (or choose scopes based on your requirements).
    - Copy the generated token and paste it into the SonarQube configuration for the Personal Access Token.
    
  After configuratoion 

  


  
  
  
  

  

  

  






