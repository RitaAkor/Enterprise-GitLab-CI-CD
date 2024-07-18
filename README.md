# Enterprise-GitLab-CI-CD
Automate enterprise software delivery with GitLab CI/CD pipeline. Deploying a  Board Game Database Full-Stack Web App on AWS EC2. Streamline code push, tool installation, testing, Docker image build, Aqua Trivy scan, and Kubernetes deployment using YAML manifests.

# Project Architecture

# The Setup Environment(Technologies)
1. Gitlab
2. Maven
3. Aqua Trivy
4. Sonarqube
5. Docker
6. Kubernetes

# Flow of Execution
* Push code to GitLab
* Install all the tools: Trivy, JDK, Docker, SonarQube, Maven
* Perform unit testing with MAVEN
* Perform dependency check using Aqua Trivy
* Perform SonarQube for quality check and code coverage
* Build the application to create an artifact with MAVEN
* Build the Docker image of the application using Docker
* Scan the Docker image using Aqua Trivy for any vulnerability
* Use YAML manifest files to deploy the application to a Kubernetes cluster

# Push code to GitLab



