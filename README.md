# CI-CD-Pipeline-Project
This project demonstrates a basic CI/CD pipeline for a Java web application using various DevOps tools and technologies. The pipeline integrates Git, GitHub, Jenkins, Maven, and Tomcat on an Ubuntu server
# Complete DevOps CI/CD Pipeline Project

This project demonstrates a basic CI/CD pipeline for a Java web application using various DevOps tools and technologies. The pipeline integrates Git, GitHub, Jenkins, Maven, and Tomcat on an Ubuntu server.

## Project Overview

The objective of this project is to:
- Set up a CI/CD pipeline for a Java web application.
- Automate the build, testing, and deployment processes.
- Deploy Jenkins on a Tomcat server running on Ubuntu.

## Tools and Technologies Used

- **Git**: Version control system to track changes in source code.
- **GitHub**: Remote repository for the Git project.
- **Maven**: Build automation tool to manage project dependencies and compile code.
- **Jenkins**: CI/CD tool to automate builds, testing, and deployment.
- **Tomcat 10**: Web server to host the Java application.
- **Ubuntu**: Operating system to host Jenkins and Tomcat.
- **Java**: Programming language for the application.

## Steps to Set Up the Project

### 1. Launch an EC2 Instance
- Sign in to your AWS Management Console.
- Launch an EC2 instance (Ubuntu 20.04 or later).
- Configure security groups to allow inbound traffic on ports 8080 (Tomcat) and 22 (SSH).

### 2. Connect to Your EC2 Instance
```bash
ssh -i <your-key-pair>.pem ubuntu@<your-ec2-public-ip>
```

### 3. Install System Dependencies
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install default-jdk -y
```

### 4. Install and Configure Tomcat 10
- Download Tomcat:
  ```bash
  wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.15/bin/apache-tomcat-10.1.15.tar.gz
  ```
- Extract and set up:
  ```bash
  sudo mkdir -p /opt/tomcat
  sudo tar xzvf apache-tomcat-*.tar.gz -C /opt/tomcat --strip-components=1
  ```
- Configure Tomcat as a service and start it:
  ```bash
  sudo nano /etc/systemd/system/tomcat.service
  sudo systemctl daemon-reload
  sudo systemctl start tomcat
  sudo systemctl enable tomcat
  ```

### 5. Install and Configure Jenkins
- Download Jenkins WAR file:
  ```bash
  wget https://get.jenkins.io/war-stable/2.346.1/jenkins.war
  ```
- Deploy Jenkins on Tomcat:
  ```bash
  sudo cp jenkins.war /opt/tomcat/webapps/
  sudo systemctl restart tomcat
  ```
- Access Jenkins at `http://<your-ec2-public-ip>:8080/jenkins`.

### 6. Install Maven
```bash
wget https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz
sudo tar xzvf apache-maven-3.9.9-bin.tar.gz -C /opt
sudo ln -s /opt/apache-maven-3.9.9 /opt/maven
export PATH=$PATH:/opt/maven/bin
mvn -version
```

### 7. Configure Jenkins Job
- **Source Code Management**: Connect to your GitHub repository.
- **Build Triggers**: Use Poll SCM or GitHub webhooks.
- **Build Steps**:
  - Invoke top-level Maven targets (e.g., `clean install`).
  - Record compiler warnings and static analysis results.
  - Publish JUnit test results.
- **Post-Build Actions**:
  - Deploy to Tomcat or Docker.
  - Send email notifications.

### 8. Configure Security and Access
- Edit `tomcat-users.xml` to add roles and users:
  ```xml
  <role rolename="admin"/>
  <role rolename="manager"/>
  <user username="admin" password="admin" roles="admin,manager"/>
  ```
- Update `context.xml` files for remote access.

### 9. Open Required Ports
```bash
sudo ufw allow 8080
```

### 10. Access the Application
- Open a browser and navigate to `http://<your-ec2-public-ip>:8080`.

## Additional Resources
- [GitHub Repository](https://github.com/tulasisahu/Ekart)
- [Jenkins Plugins](https://plugins.jenkins.io)

## Contact
For queries, email: [tksahu.devops@gmail.com](mailto:tksahu.devops@gmail.com)
