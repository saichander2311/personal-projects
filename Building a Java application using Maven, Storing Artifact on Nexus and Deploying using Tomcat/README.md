# Task: Setting Up a Multi-Server Java Application Build, Artifact Storage, and Deployment Pipeline Using Maven, Nexus, and Tomcat

## Objective

The objective of this task is to set up a multi-server environment for building, storing, and deploying a Java web application. The setup uses three separate servers: a Build Server to compile the Java Web Calculator application using Maven, a Nexus Repository Server to store the generated WAR artifact, and a Deploy Server to download and deploy the artifact on Apache Tomcat.

## Source Code

GitHub repository used for this task: Java Web Calculator App, as specified in the task requirements.

## Architecture

`GitHub → Build Server → Nexus → Deploy Server → Browser`

The implementation uses three Ubuntu-based AWS EC2 instances, with each server assigned a separate responsibility in the deployment pipeline. This separation is required by the task to simulate a real multi-server build, repository, and deployment workflow.

## Step 1: Build Server Setup (Build-VM)

- Launch EC2 instance on AWS.

![preview](./Task-1/Build-VM/VM%20name,%20AMI.png)
- Name instance as `Build-VM`.
- Select Ubuntu AMI.

![preview](./Task-1/Build-VM/Instance%20type,%20Keypair.png)
- Select instance type `t3.micro` with 2 vCPU and 1 GB memory, key pair, and SSH port 22.
- Launch instance.

![preview](./Task-1/Build-VM/Instance%20Running.png)
- Confirm instance is running.
- Connect to terminal via SSH client.

![preview](./Task-1/Build-VM/Connected%20to%20Terminal.png)
- Check if Git is installed; if not, install Git.

```bash
sudo apt install git -y
```
- Clone code from the GitHub repository.

```bash
git clone https://github.com/mrtechreddy/Java-Web-Calculator-App.git
```
![preview](./Task-1/Build-VM/Git%20clone.png)
- Install Java 17.
```bash
sudo apt install openjdk-17-jdk -y
sudo apt install maven -y
```

- Check if Java and Maven are installed.

![preview](./Task-1/Build-VM/Java,%20maven%20version.png)
- Build project with Maven.

```bash
mvn clean package
```
![preview](./Task-1/Build-VM/Build%20Success.png)
- Build success.

![preview](./Task-1/Build-VM/Artifact%20(.war).png)
- WAR file generated in the `target` directory.

## Step 2: Nexus Server Setup (Nexus-VM)

- Launch EC2 instance on AWS.
- Name instance as `Nexus-VM`.
- Select Ubuntu AMI.

![preview](./Task-1/Nexus-VM/VM%20name,%20AMI.png)
- Select instance type `c7i-flex.large` with 2 vCPU and 4 GB memory, key pair.

![preview](./Task-1/Nexus-VM/Instance%20type,%20Keypair.png)
- Configure security group rules for SSH port 22 and port 8081 (Nexus).

![preview](./Task-1/Nexus-VM/Security%20Group%20rules.png)
- Launch instance.

![preview](./Task-1/Nexus-VM/Instance%20Running.png)
- Confirm instance is running and connect via SSH client.

![preview](./Task-1/Nexus-VM/Connected%20to%20Terminal.png)
- Install Java 17.

```bash
sudo apt install openjdk-17-jdk -y
```

- Check Java version.

```bash
java -version
```

- Download and extract Nexus.

```bash
wget https://download.sonatype.com/nexus/3/nexus-3.85.0-03-linux-x86_64.tar.gz
tar -xvzf nexus-3.85.0-03-linux-x86_64.tar.gz
```

- Start Nexus.

```bash
cd nexus-3/bin
./nexus start
```
![preview](./Task-1/Nexus-VM/Starting%20Nexus.png)
- Nexus started, verify Nexus in browser using `http://<nexus-ip>:8081`.

![preview](./Task-1/Nexus-VM/Nexus%20Running%20on%20browser.png)
- Confirm Nexus is running in the browser.

![preview](./Task-1/Nexus-VM/Nexus%20Login%20Page.png)
- Open Nexus login page.
- Retrieve Nexus password from `/home/ubuntu/sonatype-work/nexus3/admin.password`.

![preview](./Task-1/Nexus-VM/Nexus%20admin.password.png)
- Log in to Nexus with username and password, then change password.

![preview](./Task-1/Nexus-VM/Nexus%20Dashboard.png)
- Open Nexus dashboard after login.
- Copy the `maven-releases` repository URL in Nexus.

## Step 3: Update `pom.xml` and `settings.xml` on Build-VM

- In pom.xml inside the project, update the repository ID and `maven-releases` repository URL.

![preview](./Task-1/Build-VM/Updated%20pom.xml.png)
- Update `settings.xml` in `/etc/maven/settings.xml` with Nexus repository ID, username, and password.

![preview](./Task-1/Build-VM/Updated%20Settings.xml.png)

## Step 4: Deploy Artifact to Nexus

```bash
mvn clean deploy
```
![preview](./Task-1/Build-VM/Deploy%20Success.png)


- Artifact deployed to Nexus repository.
- Verify in `maven-releases` repository.

![preview](./Task-1/Nexus-VM/Artifact%20in%20Nexus.png)
- WAR artifact uploaded successfully.

## Step 5: Deploy Server Setup (Deploy-VM)

- Launch EC2 instance on AWS.
- Name instance as `Deploy-VM`.
- Select Ubuntu AMI.

![preview](./Task-1/Deploy-VM/Instance%20name,%20AMI.png)
- Select instance type `t3.micro` with 2 vCPU and 1 GB memory, key pair.

![preview](./Task-1/Deploy-VM/Instance%20type,%20keypair.png)
- Configure security group rules for SSH port 22 and port 8080 (Tomcat).

![preview](./Task-1/Deploy-VM/Security%20Group%20rules.png)
- Launch instance.

![preview](./Task-1/Deploy-VM/Instance%20Running.png)
- Confirm instance is running and connect via SSH client.
- Install Java 17.
- Check Java version.
- Download Tomcat.

```bash
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.118/bin/apache-tomcat-9.0.118.tar.gz
```

- Extract Tomcat.

```bash
tar -xvzf apache-tomcat-9.0.118.tar.gz
```

- Start Tomcat.

```bash
./startup.sh
```
![preview](./Task-1/Deploy-VM/Tomcat%20Started.png)
- Verify Tomcat in browser.

![preview](./Task-1/Deploy-VM/Tomcat%20Running.png)
- Tomcat is running.
- Add credentials in `tomcat-users.xml` under `<tomcat-users>`.

![preview](./Task-1/Deploy-VM/Added%20Tomcat%20users.png)
- Download artifact into `webapps` using the artifact path in Nexus repository.

![preview](./Task-1/Deploy-VM/Artifact%20downloaded%20in%20Tomcat.png)
- Confirm artifact is downloaded.

- Refresh and run artifact file in Tomcat through the browser.

![preview](./Task-1/Deploy-VM/Application%20Deployed.png)
- Java application is deployed and running.

The application context path generally matches the WAR filename without the `.war` extension. The application was then tested by performing a calculator operation to confirm that deployment was not only reachable but also functional.

![preview](./Task-1/Deploy-VM/Application%20Running.png)

## Output

The final output of this task was a working multi-server pipeline in which the Java Web Calculator application was cloned and built on the Build Server, stored in Nexus as a WAR artifact, and then downloaded and deployed on Apache Tomcat running on the Deploy Server. This completed the required flow of build, artifact storage, and deployment using three separate virtual machines.
