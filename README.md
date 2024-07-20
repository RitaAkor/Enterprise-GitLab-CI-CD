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


<img width="1280" alt="350112976-79500fd5-8b80-43b0-a78d-78d5e2e41d9f" src="https://github.com/user-attachments/assets/8682a0c6-5f29-41cb-b3fb-73448158f4a9">

# Perform unit testing with MAVEN

To ensure that the code is functioning as expected we have to do automated tests.The "Unit Testing Stage" as shown the image below will catch any regressions or errors early in the development process before the code is deployed. Click on the commit change and wait for the results by viewing on the pipeline. This will install all the tools defined above.

<img width="1280" alt="350115222-ca3e691f-677c-4442-980a-5833d42a2cb2" src="https://github.com/user-attachments/assets/0cfdb039-3d84-43f0-922a-0fa77d25b877">

The completion image is below:

<img width="1280" alt="350115912-dff9c7ec-bd8b-46b1-b9f5-fdad6955d4d1" src="https://github.com/user-attachments/assets/a1963ee0-40fa-445e-a890-b6d41a50311b">

# Perform dependency check using Aqua Trivy

The trivy_fs_scan stage in the CI/CD pipeline is designed to perform a file system security scan using Trivy, a vulnerability scanner. This section in the build stage,specifies that the trivy_fs_scan job belongs to the security stage of your pipeline. The security stage is dedicated to tasks related to scanning and analyzing security vulnerabilities.The script section contains the commands that will be executed during this stage. trivy fs --format table -o fs.html .: This command runs Trivy to scan the file system for vulnerabilities. trivy fs: Invokes Trivy to perform a file system scan. --format table: Specifies the output format of the scan results. In this case, the results will be displayed in a table format. -o fs.html: Directs Trivy to save the scan results to a file named fs.html in HTML format. This allows for easy viewing and sharing of the scan results. .: Indicates the current directory as the target for the scan. Trivy will scan the entire file system starting from this directory


<img width="1280" alt="350119680-252680a7-89e5-4fdd-a783-0b6e8d391552" src="https://github.com/user-attachments/assets/8a7941c6-ff50-4dc7-8765-9ed7546f17d8">

# Perform SonarQube for quality check and code coverage


For sonarqube,the first thing to do since docker is already installed on the terminal through the pipeline is to run this command: `Docker run -d -p 9000:9000 nsonarqube:lts-community. The docker run: Starts a new Docker container. -d: Runs the container in the background (detached mode). -p 9000:9000: Maps port 9000 of the container to port 9000 on your host machine, making SonarQube accessible at http://localhost:9000. sonarqube:lts-community: Specifies the Docker image and tag for SonarQube’s LTS Community Edition.
To access SonarQube, copy the IP address of the EC2 instance (named corporate-dev) from AWS, paste it into your browser, and append the port 9000. SonarQube will start, and you'll be able to configure it so that reports can be pushed to the SonarQube server through the pipeline. Configuration Steps; - Log In: Access SonarQube and log in with the username admin and password admin. Change the password to something secure. - Setup Integration with GitLab: - Go to the SonarQube settings. - Choose the GitLab option. - Enter a configuration name of your choice. - For GitLab API URL, use: https://gitlab.com/api/v4. - Generate a Personal Access Token: - Navigate to GitLab, then go to Settings > Access Tokens. - Click on Add New Token. - Provide a name for the token. - Select the Reporter role under Scopes (or choose scopes based on your requirements). - Copy the generated token and paste it into the SonarQube configuration for the Personal Access Token.
After configuration,it will prmpt options to analyze your project,i used the Gitlab CI.Create a sonar.project.properties and add the provided command on the file. 

<img width="1280" alt="350129630-7e172ef2-240f-4856-adba-628cc2eb025e" src="https://github.com/user-attachments/assets/d5e4f004-0ddf-4e3e-b0e0-7db4748f3f23">

The next step is to add environment variables by creating two tokens.The first SonarQube Token and SOnarQube Host Url.Uncheck the protect variable ehile this is been created. This will automatically generate the code for the sonarqube build stage as seen below;


<img width="1280" alt="350131038-bd4377de-ab20-4e1b-bb38-d7659c9ad13b" src="https://github.com/user-attachments/assets/b058537a-0d99-47ca-bce5-9a41e9924fb3">
Commit stage,see status shown below.


<img width="1280" alt="350131307-f609dbff-7328-4266-be3a-329e1eff85ac" src="https://github.com/user-attachments/assets/3efe7ea1-541f-4cf2-b866-2e2e917ec1f9">



<img width="1280" alt="350131679-83674e7e-c8f3-4005-8b3e-8894f1caa222" src="https://github.com/user-attachments/assets/af3aee8c-5a93-41d0-b308-096d8329320a">

# Build the Docker image of the application using Docker

In this stage, we will create a script for logging in by using Docker credentials. Instead of hardcoding these credentials directly into the script, we will define them as environment variables for security reasons.
To do this:
Navigate to Settings > CI/CD in your project. Add the necessary variables (e.g., DOCKER_USERNAME and DOCKER_PASSWORD) with their respective values (username and password). This approach ensures that sensitive information is securely managed. Include mvn package in the script,because the artifcate might not be available to docker Run the command:Docker build -t for taging (your username) and tag- This is show below for understanding. Finally Pushing the image

<img width="1273" alt="350134236-eec4df73-cb28-40f7-bc62-c40707129744" src="https://github.com/user-attachments/assets/bcd9b8b0-d0b8-4764-a2bb-5c07dad0e66a">
<img width="1278" alt="350135857-ca0ea55a-d6b8-46d6-a7b9-97aed7e3cc2a" src="https://github.com/user-attachments/assets/a97b8f91-7e5d-4f5a-9c60-fb2b731aa1ee">

Commit the changes to see status,see result below;
<img width="1280" alt="350136381-1c649745-75a1-483e-84cc-f5d9bda3abc9" src="https://github.com/user-attachments/assets/0132d008-3365-4368-a3b8-dda76bd2cc53">

# Scan the Docker image using Aqua Trivy for any vulnerability

In this stage, I set up a self-hosted Kubernetes cluster using two Ubuntu virtual machines on AWS, named k8-master and k8-slave. After launching the instances,i connected via Tabby terminal, update the packages with sudo apt update, and install Kubernetes components (kubectl, kubelet, kubeadm) on all nodes.Setting up a self-hosted Kubernetes cluster allows for managing containerized applications across multiple nodes. kubectl, kubelet, and kubeadm are essential tools for managing and maintaining the cluster.
First, create a file with any name (e.g., 1.sh), make it executable, and paste the commands for downloading kubectl, kubelet, and kubeadm obtained from the Kubernetes documentation website.See image below;

<img width="798" alt="350139313-dc4119ef-cc1d-49bb-b42b-ee24cc099d0e" src="https://github.com/user-attachments/assets/90fea278-dbce-4c9f-a9df-67ad28fd504a">

Additionally, to connect the slave machine to the master node, run the command sudo kubeadm init --pod-network-cidr=10.244.0.0/16 on the master node. This command initializes the Kubernetes cluster and generates a token used for authenticating the connection of the slave machine to the master node.
Next step,is to create a directory on the .kube folder change permission: sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config To change ownership: sudo chown $(id -u):$(id -g) $HOME /.kube/config
Finally, convert the kubeconfig file into a Base64-encoded format. Use the cat kubeconfig command to display its contents, then paste the output into the echo -n '' command and run it.

# Use YAML manifest files to deploy the application to a Kubernetes cluster

In the final stage, go to the CI/CD settings, navigate to the build stage, and create a kubeconfig path. Create a folder and add a variable named KUBECONFIG_CONTENT. Copy the content of the kubeconfig file from the terminal and paste it into the GitLab build section under this variable.Replace docker image to the docker image created. See attached image:
<img width="1280" alt="350142864-03d344f9-724a-4f7e-96d7-c3b11e3399d6" src="https://github.com/user-attachments/assets/ffa4240c-239f-4649-81c2-909865243055">

Copy IP address and port number.Finally,what it looks like below!
<img width="1280" alt="350148790-7b9f1052-ee69-46d0-975f-6a9a83487d13" src="https://github.com/user-attachments/assets/1849b40c-cbe8-450a-81af-39f1f2f25052">








