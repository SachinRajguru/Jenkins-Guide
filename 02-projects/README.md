
# 📄 `jenkins-installation + docker-agent-setup`

## Jenkins Installation on AWS EC2

### 1. Launch EC2 Instance

* Go to **AWS Console**
* Navigate to **EC2 → Instances**
* Click **Launch Instance**
* Choose Ubuntu (recommended)

<img width="994" alt="Launch EC2 Instance" src="https://user-images.githubusercontent.com/43399466/215974891-196abfe9-ace0-407b-abd2-adcffe218e3f.png">

### 2. Install Jenkins – Prerequisites

#### Install Java (JDK 17)

```bash
sudo apt update
sudo apt install openjdk-17-jre
```

#### Verify Java Installation

```bash
java -version
```

### 3. Install Jenkins

```bash
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins
```

### 4. Configure AWS Security Group (IMPORTANT)

By default, Jenkins is not accessible externally.

#### Open Port 8080:

* Go to **EC2 → Instances**
* Select your instance
* Scroll to **Security → Security Groups**
* Add **Inbound Rule**:

  * Type: Custom TCP
  * Port: 8080
  * Source: Anywhere (0.0.0.0/0) *(or restrict as needed)*

> ⚠️ Best Practice: Avoid "All Traffic" in production. Allow only port **8080**.

<img width="1187" alt="Security Group Rule" src="https://user-images.githubusercontent.com/43399466/215975712-2fc569cb-9d76-49b4-9345-d8b62187aa22.png">

### 5. Access Jenkins

Open in browser:

```bash
http://<ec2-instance-public-ip-address>:8080
```

### 6. Unlock Jenkins

Run on EC2:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

* Copy the password
* Paste into Jenkins UI

<img width="1291" alt="Unlock Jenkins" src="https://user-images.githubusercontent.com/43399466/215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3.png">

### 7. Initial Setup

#### Install Plugins

* Click **Install Suggested Plugins**

<img width="1291" alt="Install Plugins" src="https://user-images.githubusercontent.com/43399466/215959294-047eadef-7e64-4795-bd3b-b1efb0375988.png">

* ⏳ Wait for installation to complete

<img width="1291" alt="Plugin Installation Progress" src="https://user-images.githubusercontent.com/43399466/215959398-344b5721-28ec-47a5-8908-b698e435608d.png">

#### Create Admin User

* Create user (recommended for long-term use)
* Or skip if temporary setup

<img width="990" alt="Create Admin User" src="https://user-images.githubusercontent.com/43399466/215959757-403246c8-e739-4103-9265-6bdab418013e.png">

### 8. Jenkins Ready

<img width="990" alt="Jenkins Dashboard" src="https://user-images.githubusercontent.com/43399466/215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7.png">

- ✅ Jenkins installation is now complete
- ✅ You can start creating pipelines

### 9. Install Required Plugin (Docker Pipeline)

* Go to **Manage Jenkins → Manage Plugins**
* Open **Available tab**
* Search: `Docker Pipeline`
* Install and **Restart Jenkins**

<img width="1392" alt="Docker Pipeline Plugin" src="https://user-images.githubusercontent.com/43399466/215973898-7c366525-15db-4876-bd71-49522ecb267d.png">

## Docker Agent Setup for Jenkins

### 1. Install Docker on EC2

```bash
sudo apt update
sudo apt install docker.io
```

### 2. Grant Permissions

Add both **jenkins** and **ubuntu** users to Docker group:

```bash
sudo su -
usermod -aG docker jenkins
usermod -aG docker ubuntu
```

### 3. Restart Docker

```bash
systemctl restart docker
```

### 4. Restart Jenkins

After permission changes, restart Jenkins:

```bash
http://<ec2-instance-public-ip>:8080/restart
```

### 5. Verification

* Jenkins can now execute Docker commands
* Docker-based pipelines (agents) will work
* No need for sudo inside pipelines

### 6. Final Outcome

- ✅ Docker installed and configured
- ✅ Jenkins integrated with Docker daemon
- ✅ Ready for:
    - CI/CD pipelines
    - Docker builds
    - Containerized deployments

## What’s Next?

You are now ready to:
* Create Jenkins Pipelines
* Build and manage Docker images
* Push images to container registries
* Deploy applications to Kubernetes (K8s)
