# Task: SonarQube Installation & Java Application Code Analysis
## Objective:
Install and configure SonarQube on a Linux server, connect a Java Maven application, and perform static code analysis using SonarQube.
## Prerequisites
OS - Ubuntu 24.04 LTS / Amazon Linux 2, RAM - Minimum 4 GB (8 GB recommended), Java - OpenJDK 17, Build Tool - Maven

## Step-1: Setup Application Build Server (Build-VM)
- Launch Ec2 instance on AWS

![preview](./Screenshots/Build-VM/VM%20name,%20AMI.png)
- Name instance as `Build-VM`

- Select ubuntu AMI

![preview](./Screenshots/Build-VM/Instance%20type,%20Keypair.png)
- Select Instance type `t3.micro` with 2vCPU and 1GB memory, keypair

![preview](./Screenshots/Build-VM/Security%20Group%20rules%20SSH-HTTP.png)
- Security Group rules as SSH port 22 and HTTP port 80 and launch Instance

![preview](./Screenshots/Build-VM/Instance%20Running.png)
- Instance Running

![preview](./Screenshots/Build-VM/Connected%20to%20terminal.png)
- Connected to terminal via SSH client

- Check if Git is installed, by running command git

![preview](./Screenshots/Build-VM/check%20if%20git%20installed.png)

- If git is not installed, install git
```bash
sudo apt install git -y
```
- clone code from GitHub repository
```bash
git clone  https://github.com/mrtechreddy/JavaWebCalculator.git
```
![preview](./Screenshots/Build-VM/Clone%20code.png)
- Install Java 17 and maven
```bash
sudo apt install openjdk-17-jdk maven -y
```
![preview](./Screenshots/Build-VM/Install%20java%20and%20maven.png)
- Check if Java and Maven are installed
```bash
java –version
mvn --version
```
![preview](./Screenshots/Build-VM/Java,%20maven%20version.png)










## Step-2: Setup Test Server (Sonar-VM)
- Launch Ec2 instance on AWS

![preview](./Screenshots/Test-VM/VM%20name,%20AMI.png)
- Name instance as `Sonar-VM`

- Select ubuntu AMI

![preview](./Screenshots/Test-VM/Instance%20Type,%20Keypair.png)
- Select Instance type `c7i-flex.large` with 2vCPU and 4GB memory, keypair

![preview](./Screenshots/Test-VM/Security%20Group%20rules,%20SSH-SONAR.png)
- Security Group rules as SSH port 22 and Sonar port 9000 and launch Instance

![preview](./Screenshots/Test-VM/Instance%20Running.png)
- Instance Running

![preview](./Screenshots/Test-VM/Connected%20to%20Terminal%20and%20update.png)
- Connected to terminal via SSH client and run updates

- Install Java 17
```bash
sudo apt install openjdk-17-jdk -y
```
![preview](./Screenshots/Test-VM/Installing%20Java.png)
- Check java version
```bash
java --version
```
![preview](./Screenshots/Test-VM/Java%20version.png)
- Download SonarQube
```bash
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.6.0.92116.zip
```
![preview](./Screenshots/Test-VM/Donwloading%20SonarQube.png)
- Install unzip to unzip the downloaded zip file
```bash
sudo apt install unzip -y
```
![preview](./Screenshots/Test-VM/install%20unzip.png)
- Unzip downloaded sonarqube zip file
```bash
sudo unzip sonarqube-10.6.0.92116.zip
```
![preview](./Screenshots/Test-VM/unzip%20SonarQube.png)
- Files after unzipping downloaded sonarqube zip file

![preview](./Screenshots/Test-VM/Files%20after%20unzipping%20Sonarqube.png)
- change the ownership of the SonarQube directory so that the ubuntu user and ubuntu group have full access to prevent permission errors.
```bash
sudo chown -R ubuntu:ubuntu ~/sonarqube-10.6.0.92116
```
![preview](./Screenshots/Test-VM/change%20ownership%20of%20sonarqube.png)
- Start SonarQube and check status
```bash
./sonar.sh start
./sonar.sh status
```
![preview](./Screenshots/Test-VM/start%20sonarqube,%20running.png)


## Step-3: SonarQube on browser
- SonarQube is running successfully on browser

![preview](./Screenshots/Test-VM/Sonarqube%20is%20running%20on%20browser.png)

- Enter credentials of SonarQube

- Default username and password are admin:admin

![preview](./Screenshots/Test-VM/Login%20sonarqube%20(default).png)
- After log in change password

![preview](./Screenshots/Test-VM/SonarQube%20dashboard.png)

- Dashboard of SonarQube after log in

`Go to Administration -> Security -> Users`

![preview](./Screenshots/Test-VM/Administration,%20Security,%20Users.png)
![preview](./Screenshots/Test-VM/Users%20dashboard.png)

- In Users dashboard select token to generate token

- Enter token name, select expiry date and select Generate

![preview](./Screenshots/Test-VM/Generating%20Token.png)
- Copy the Generated token




- Copy SonarQube IP-address

![preview](./Screenshots/Test-VM/Copy%20sonarqube%20ip-address.png)
- Token and IP-address will be used to run maven goal for testing
## Step-4: Testing Java code using maven and publishing report on SonarQube
- Run Sonar Analysis

- Give project name at projectKey

- Paste SonarQube IP-address with port number

- Paste Generated Token at login

```bash
mvn clean verify sonar:sonar \
-Dsonar.projectKey=my-java-app \
-Dsonar.host.url=http://100.26.106.242:9000 \
-Dsonar.login=squ_556715dce2414714e0f0e929afaa22c74447f3ee
```

![preview](./Screenshots/Test-VM/Run%20maven%20goal%20with%20updated%20token%20and%20ip-address%20in%20build%20VM.png)

![preview](./Screenshots/Test-VM/Build%20success%20and%20report%20is%20uploaded%20to%20sonarqube.png)
- Build is success

- Results, analysis are published to SonarQube

- Report is uploaded to SonarQube under Projects

![preview](./Screenshots/Test-VM/Report%20is%20uploaded%20to%20SonarQube.png)
- Open report to check Quality Gate dashboard

![preview](./Screenshots/Test-VM/Quality%20Gate%20Dashboard.png)
- Quality Gate dashboard

- Can check all the Quality Gate issues in here

- Maintainability in SonarQube measures how easy it is to understand, repair, modify, or extend your code.

![preview](./Screenshots/Test-VM/Issue%20in%20Maintainability.png)
- Can check issues in-depth when it is opened

![preview](./Screenshots/Test-VM/Issue%20in%20Detail.png)
## Why Static Analysis Security Testing (SAST)
Static Analysis Security Testing (SAST) is used to identify security vulnerabilities, coding issues, and weaknesses in an application's source code before the application is deployed or executed or compiled.
## Conclusion
Successfully installed SonarQube, configured SonarQube, integrated a Maven-based Java application and performed static code analysis to identify code quality issues and vulnerabilities.