# Jenkins-Guide

## Lab Manual & Technical Study Guide

**Objective:** Master Jenkins installation, configuration, Docker integration, and CI/CD pipeline setup through hands-on lab exercises.

**Prerequisites:** AWS account, basic Linux command knowledge

 **Learning Outcomes:**

    Install Jenkins on AWS EC2 with production-grade security
    Configure Docker agents for scalable builds
    Understand Jenkins architecture and security best practices
    Prepare for real-world CI/CD pipeline deployment

---

## Lab 1: AWS EC2 Instance Setup & Jenkins Installation

### Pre-Lab Theory: Why EC2 + Jenkins?

**Analogy:** Think of Jenkins as your "orchestrator conductor" in a symphony orchestra. EC2 provides the "stage (infrastructure)" where Jenkins performs CI/CD magic.

 **Learning Objectives:**

    Launch EC2 instance and configure security groups
    Install Java JDK and Jenkins from official repositories
    Access Jenkins dashboard securely
    Complete initial Jenkins setup wizard

**Real-World Scenario:** You're a DevOps engineer tasked with setting up a CI/CD server for a microservices application that needs to build Docker images and deploy to Kubernetes.

Companies like Netflix, Airbnb use Jenkins on EC2 to automate deployments to thousands of microservices daily.

### Step 1.1: Launch EC2 Instance

**Analogy**: Think of EC2 as your personal cloud computer lab where Jenkins will run as the main experiment controller.

**Lab Procedure:**

1. Login to AWS Console → EC2 Dashboard
2. Click "Launch Instances" 
3. Select Ubuntu Server 20.04 LTS (Free Tier Eligible)
4. t2.micro instance type → Review and Launch
5. Create/Select Key Pair → Launch

**Technical Definition**: EC2 (Elastic Compute Cloud) provides scalable virtual servers in the cloud.

**Visual Checkpoint:**
<img width="994" alt="Screenshot 2023-02-01 at 12 37 45 PM" src="https://user-images.githubusercontent.com/43399466/215974891-196abfe9-ace0-407b-abd2-adcffe218e3f.png">

**Pro Tip:** Select Ubuntu Server 20.04 LTS (t2.micro - free tier eligible) with at least 2GB RAM for smooth Jenkins operation.

**Interview Question:** Why Ubuntu over Amazon Linux for Jenkins?
**Answer:** Ubuntu has better Java/Docker package support and larger community plugins ecosystem.

### Step 1.2: Install Java (JDK) - Jenkins Prerequisite

**Definition:** Jenkins requires Java Runtime Environment (JRE) to execute Groovy scripts and manage build agents.

```bash
# Update package index
sudo apt update

# Install OpenJDK 17 (Jenkins recommended version)
sudo apt install openjdk-17-jre -y

# Verify installation
java -version
```

**Expected Output:**

```bash
openjdk version "17.0.x" 2023-xx-xx
OpenJDK Runtime Environment (build 17.0.x+x)
```

**Troubleshooting:** If `java -version` fails, run `sudo apt --fix-broken install`. Else check if multiple Java versions exist: `sudo update-alternatives --config java`

### Step 1.3: Install Jenkins (Production Method)

**Technical Note:** We're using Jenkins official Debian repository for latest stable version with GPG key verification. Using official Jenkins APT repository ensures you get the latest stable version with security updates.

```bash
# Add Jenkins GPG key i.e. Add Jenkins official key for package verification
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

# Add Jenkins repository to APT sources
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

# Update package index and install Jenkins
sudo apt-get update
sudo apt-get install jenkins -y
```

**Service Status Check:**

```bash
sudo systemctl status jenkins
# Expected: Active (running)
```

### Step 1.4: Configure AWS Security Group (Critical Security Step)

**Analogy**: Security Groups act as firewall rules for your EC2 instance - like door locks for your lab.
**Security Best Practice**: NEVER allow "All Traffic" in production. Use specific ports only.

**Security Configuration: Open Port 8080**

**Lab Procedure:**

1. EC2 Dashboard → Instances → Select your Instance ID
2. Bottom tabs → Security → Security Groups → Edit Inbound Rules
3. Add Rule:

| Type       | Protocol | Port Range | Source       |
|------------|----------|------------|--------------|
| Custom TCP | TCP      | 8080       | 0.0.0.0/0 (or your IP) |
| SSH        | TCP      | 22         | Your IP only |

**Visual Checkpoint:**
<img width="1187" alt="Screenshot 2023-02-01 at 12 42 01 PM" src="https://user-images.githubusercontent.com/43399466/215975712-2fc569cb-9d76-49b4-9345-d8b62187aa22.png">

**Interview Question:** Why not allow "All Traffic"?
**Answer:** Principle of least privilege. Port 8080 only needed for Jenkins web UI. "All Traffic" exposes SSH (22), Docker ports unnecessarily.

### Step 1.5: First Jenkins Login & Setup

**Access URL:** http://<EC2-Public-IP>:8080
**Public IP Location**: EC2 Console → Instance Details → Public IPv4 address

**Retrieve Admin Password:**

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

**Lab Walkthrough:**

1. Paste the 32-character password
2. Click "Install suggested plugins" 
3. Wait for plugin installation (3-5 minutes)
4. Create Admin User (Recommended for production)

**Visual Checkpoints:**

1. Password Screen:
<img width="1291" alt="Screenshot 2023-02-01 at 10 56 25 AM" src="https://user-images.githubusercontent.com/43399466/215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3.png">

Plugin Installation: Install Suggested Plugins (Recommended)
<img width="1291" alt="Screenshot 2023-02-01 at 10 58 40 AM" src="https://user-images.githubusercontent.com/43399466/215959294-047eadef-7e64-4795-bd3b-b1efb0375988.png">

Plugin Installation Progress: 
<img width="1291" alt="Screenshot 2023-02-01 at 10 59 31 AM" src="https://user-images.githubusercontent.com/43399466/215959398-344b5721-28ec-47a5-8908-b698e435608d.png">

Admin User Setup: Create Admin User (Recommended for production)
<img width="990" alt="Screenshot 2023-02-01 at 11 02 09 AM" src="https://user-images.githubusercontent.com/43399466/215959757-403246c8-e739-4103-9265-6bdab418013e.png">

Success Screen: Jenkins Dashboard Ready
<img width="990" alt="Screenshot 2023-02-01 at 11 14 13 AM" src="https://user-images.githubusercontent.com/43399466/215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7.png">

**Interview Question:** Q: What plugins are installed in "Suggested Plugins"? 
**Answer:** Essential plugins including Git, Pipeline, Blue Ocean, Credentials Binding, and build tools - providing 80% of common CI/CD functionality out-of-box.

✓ **Lab 1 Complete**: Jenkins is running securely on EC2!

---

## Lab 2: Docker Pipeline Plugin Installation

### Pre-Lab Theory: Why Docker Pipeline Plugin?

**Definition:** Docker Pipeline plugin enables "Docker-as-code" in Jenkinsfiles, allowing dynamic containerized build environments.

**Analogy:** Like having a "temporary workshop" (Docker container) for each build that auto-cleans after use.

**Learning Objectives**

    Install Docker Pipeline plugin for containerized builds
    Understand plugin architecture in Jenkins ecosystem

**Real-World Scenario**: Your team builds microservices in Docker containers. Jenkins needs Docker Pipeline plugin to execute `docker build`, `docker run` commands in pipelines.

### Step 2.1: Install Docker Pipeline Plugin

**Step-by-Step Procedure:**

1. Jenkins Dashboard → Manage Jenkins → Manage Plugins
2. "Available" tab → Search "Docker Pipeline"
3. Check box → Install without restart
4. Wait for download → Jenkins auto-restarts

**Visual Checkpoint:**
<img width="1392" alt="Screenshot 2023-02-01 at 12 17 02 PM" src="https://user-images.githubusercontent.com/43399466/215973898-7c366525-15db-4876-bd71-49522ecb267d.png">

**Post-Installation:** Jenkins auto-restarts (wait 1-2 minutes)

**Verification:**

Manage Jenkins → Manage Plugins → Installed → Search "Docker Pipeline" ✓


**Interview Question:** What does Docker Pipeline plugin provide over basic Docker support?
**Answer:** Native `docker.*` step syntax in Jenkinsfile (ex: `docker.build()`, `docker.run()`), agent templating, and Docker volume mounts.

**Interview Question:** Q: Why use Docker Pipeline plugin instead of plain Docker commands? 
**Answer:** Provides Jenkinsfile DSL syntax (`docker.image().inside()`), agent management, credential handling, and integrates with Jenkins controller architecture for distributed builds.

---

## Lab 3: Docker Slave/Agent Configuration

### Pre-Lab Theory: Jenkins Master-Agent Architecture

**Learning Objectives**

    Install Docker on Jenkins host
    Configure Jenkins user permissions for Docker daemon
    Enable Docker-in-Docker (DinD) for build agents

**Technical Definition**: Docker Slave/Agent allows Jenkins to spin up ephemeral Docker containers as build environments, providing isolation and scalability.

**Analogy**: Docker containers are like disposable lab workstations - clean, identical, and automatically cleaned up after experiments.

**Real-World Scenario:** Single Jenkins master can't handle 100+ parallel builds. Docker agents scale horizontally.

**Architecture Diagram Concept:**

```bash
Jenkins Master (EC2) ←→ Docker Agents (Containers)
         ↓                    ↓
     Web UI + Orchestration   Build/Tests in Isolation
```

### Step 3.1: Install Docker on Jenkins Host

```bash
# Update packages
sudo apt update

# Install Docker Community Edition
sudo apt install docker.io
```

**Verify Installation:**

```bash
sudo docker --version
sudo docker run hello-world  # Test container runtime
```

### Step 3.2: Grant Permissions to Jenkins User

**Critical Security Step:** Jenkins process needs Docker socket access without sudo.

**Security Context**: Jenkins runs as 'jenkins' user. Docker daemon requires group membership for non-root access.

```bash
# Switch to root
sudo su -

# Add jenkins user to docker group
usermod -aG docker jenkins

# Add ubuntu user (for manual testing)
usermod -aG docker ubuntu

# Restart Docker daemon
systemctl restart docker

# Exit root
exit
```

**Apply Group Changes (relogin or):**

```bash
newgrp docker
```

**Technical Note:** Group membership requires logout/login or EC2 reboot for jenkins user.

### Step 3.3: Restart Jenkins Service

```bash
# Access restart URL (safer than systemctl)
http://<EC2-Public-IP>:8080/restart

# OR via CLI
sudo systemctl restart jenkins
```

**Verification Test:**

```bash
# Test as jenkins user
sudo -u jenkins docker version
```

✓ **Lab 3 Complete: Docker agent configuration successful!**

---

## Post-Lab Assessment & Next Steps

### Verification Checklist

    [ ] Jenkins dashboard accessible on port 8080
    [ ] Docker Pipeline plugin installed and verified
    [ ] `jenkins` user in `docker` group
    [ ] `docker version` works without sudo
    [ ] Security group allows only necessary ports

### Common Interview Questions & Answers

**Q1:** Why install Jenkins on EC2 instead of local machine? **A:** EC2 provides high availability, auto-scaling, backup/restore, integration with AWS services (CloudWatch, EKS), and team access. Local machines lack persistence and scalability.

**Q2:** What's the difference between Jenkins controller and agent? **A:** Controller manages jobs, plugins, UI. Agents execute actual builds in isolated environments (Docker containers, VMs). Scales horizontally.

**Q3:** How does Docker-in-Docker work in Jenkins? **A:** Jenkins agent container mounts host Docker socket (/var/run/docker.sock), allowing containerized builds to spawn child containers using host Docker daemon.

**Q4:** How does Jenkins Docker agent improve build isolation?
**A:** Each build runs in fresh container. No dependency conflicts, auto-cleanup, reproducible environments.

**Q5:** Why use official Jenkins repository over .war file?
**A:** Auto-updates, dependency management, systemd integration vs manual .war upgrades.

**Q6:** Security implications of adding Jenkins to docker group?
**A:** Jenkins gains root-equivalent Docker access. Mitigate with Docker Content Trust, read-only mounts, network policies.

### Next Lab Preview

**Lab Status:** ✓ Jenkins + Docker Agent Ready for Pipelines!

**Next Lab:** Building end-to-end CI/CD pipelines with Kubernetes deployment

---