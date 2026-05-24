# Task: JavaScript Application Code Analysis using SonarQube

## Objective:

Install and configure SonarQube on a Linux server, connect an Angular Calculator application written in JavaScript, and perform static code analysis using npm-based SonarQube integration.

## Prerequisites

OS - Ubuntu 24.04 LTS / Amazon Linux 2, RAM - Minimum 4 GB (8 GB recommended), Node.js - Version 18 or above, Build tool - npm, Java -		OpenJDK 17, Angular CLI - Latest, SonarQube - 10.x


## Step-1: Setup Application Build Server (Build-VM)
- Launch Ec2 instance on AWS 

![preview](./Build-VM/VM%20name,%20AMI.png)
- Name instance as Build-VM
- Select ubuntu AMI 

![preview](./Build-VM/Instance%20type,%20Keypair.png)
- Select Instance type t3.micro with 2vCPU and 1GB memory, keypair 

![preview](./Build-VM/Security%20Group%20rules%20SSH-HTTP.png)
-	Security Group rules as SSH port 22 and HTTP port 80 and launch Instance

![preview](./Build-VM/Instance%20Running.png)
-	Instance Running 

![preview](./Build-VM/Connected%20to%20Terminal.png)
-	Connected to terminal via SSH client

-	Check if Git is installed, by running command git 

![preview](./Build-VM/check%20if%20git%20installed.png)
-	If git is not installed, install git

```bash 
sudo apt install git -y
```
-	clone code from GitHub repository
```bash
git clone https://github.com/saichander2311/AngularCalculator.git
```
![preview](./Build-VM/Code%20Cloned.png)
-	Files in cloned repository AngularCalculator

![preview](./Build-VM/files%20in%20AngularCalculator.png)
-	Install Node.js and npm
```bash
sudo apt install nodejs npm -y
```
![preview](./Build-VM/installing%20nodejs%20and%20npm.png)
-	Check node and npm versions
```bash
node -v
npm -v
```
![preview](./Build-VM/node,%20npm%20version.png)
-	Install angular cli
```bash
sudo npm install -g @angular/cli
```
![preview](./Build-VM/install%20angular%20cli.png)
-	Verify Angular CLI
```bash
ng version
```
![preview](./Build-VM/ng%20version.png)

## Step-2: Setup SonarQube Server (Sonar-VM)
-	Launch Ec2 instance on AWS

![preview](./Sonar-VM/VM%20name,%20AMI.png)
-	Name instance as Sonar-VM
-	Select ubuntu AMI 

![preview](./Sonar-VM/Instance%20Type,%20Keypair.png)
-	Select Instance type c7i-flex.large with 2vCPU and 4GB memory, keypair 

![preview](./Sonar-VM/Security%20Group%20rules,%20SSH-SONAR.png)
-	Security Group rules as SSH port 22 and Sonar port 9000 and launch Instance

![preview](./Sonar-VM/Instance%20Running.png)
-	Instance Running 

![preview](./Sonar-VM/Connected%20to%20Terminal%20and%20update.png)
-	Connected to terminal via SSH client and run updates
-	Install Java 17
```bash
sudo apt install openjdk-17-jdk -y
```
![preview](./Sonar-VM/Installing%20Java.png)
-	Check java version
```bash
java –version
```
![preview](./Sonar-VM/Java%20version.png)

-	Download SonarQube
```bash
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.6.0.92116.zip
```
![preview](./Sonar-VM/Downloading%20SonarQube.png)
-	Install unzip to unzip the downloaded zip file
```bash
sudo apt install unzip -y
```
![preview](./Sonar-VM/install%20unzip.png)
-	Unzip downloaded sonarqube zip file
```bash
sudo unzip sonarqube-10.6.0.92116.zip
```
![preview](./Sonar-VM/unzip%20SonarQube.png)

-	Files after unzipping downloaded sonarqube zip file

![preview](./Sonar-VM/Files%20after%20unzipping%20Sonarqube.png)
-	change the ownership of the SonarQube directory so that the ubuntu user and ubuntu group have full access to prevent permission errors.
```bash 
sudo chown -R ubuntu:ubuntu ~/sonarqube-10.6.0.92116
```
![preview](./Sonar-VM/change%20ownership%20of%20sonarqube.png)

-	Start SonarQube and check status
```bash
./sonar.sh start
./sonar.sh status
```
![preview](./Sonar-VM/start%20sonarqube,%20running.png)
## Step-3: SonarQube on browser
-	SonarQube is running successfully on browser

![preview](./Sonar-VM/Sonarqube%20is%20running%20on%20browser.png)
-	Enter credentials of SonarQube 
-	Default username and password are admin:admin

![preview](./Sonar-VM/Login%20sonarqube%20(default).png)

-	After log in change password 

![preview](./Sonar-VM/SonarQube%20dashboard.png)
-	Dashboard of SonarQube after log in

`Go to Administration -> Security -> Users` 

![preview](./Sonar-VM/Administration,%20Security,%20Users.png)
![preview](./Sonar-VM/Users%20dashboard.png)
- In Users dashboard select token to generate token
-	Enter token name, select expiry date and select Generate

![preview](./Sonar-VM/Generating%20Token.png)
-	Copy the Generated token
-	Copy SonarQube IP-address

![preview](./Sonar-VM/Copy%20sonarqube%20ip-address.png)
## Step-4: Configure Angular Project for SonarQube
-	Install SonarQube Scanner Package
-	Inside Angular project:
```bash
npm install sonarqube-scanner --save-dev
```
![preview](./Build-VM/install%20sonarqube-scanner.png)
-	Create sonar-project.properties file inside cloned repository where src/, package.json, angular.json are present and paste this code
```bash
const sonarqubeScanner = require('sonarqube-scanner').default;

sonarqubeScanner(
  {
    serverUrl: 'http://<Sonar-IP>:9000',
    token: 'Sonar-token',
    options: {
      'sonar.projectName': 'Angular-Calculator-App',
      'sonar.projectKey': 'angular-calculator-app',
      'sonar.sources': 'src',
      'sonar.sourceEncoding': 'UTF-8',
    }
  },
  () => process.exit()
);
```
![preview](./Build-VM/sonar-project.properties%20code.png)

## Step-5: Run Static Code Analysis
-	Execute SonarQube Analysis
```bash 
node sonar-project.properties
```
![preview](./Build-VM/Run%20analysis.png)
-	Analysis successful and reports are published

![preview](./Build-VM/Analysis%20Successful.png)

## Step-6: Verify Analysis Results
-	Report is uploaded to SonarQube under projects

![preview](./Sonar-VM/report%20is%20uploaded%20to%20SonarQube.png)
-	Open report to check Quality Gate dashboard

![preview](./Sonar-VM/Quality%20Gate%20dashboard.png)
-	Quality Gate dashboard

![preview](./Sonar-VM/Reliability.png)
-	Reliability measures how dependable your software is and evaluates whether your code can consistently function without errors, crashes, or unexpected behaviours under stated conditions

![preview](./Sonar-VM/issue%20in%20detail.png)
-	Issues in details

## Why Static Analysis Security Testing (SAST)
Static Analysis Security Testing (SAST) is used to identify security vulnerabilities, coding issues, and weaknesses in an application's source code before the application is deployed or executed or compiled
## Conclusion
Successfully installed SonarQube, Configured Angular application, Integrated npm-based SonarQube analysis, performed static code analysis and published reports to SonarQube dashboard 
