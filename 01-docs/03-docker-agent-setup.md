
# 📄 `03-docker-agent-setup.md`

## Docker Agent Setup for Jenkins

### 1. Install Docker on EC2

```bash
sudo apt update
sudo apt install docker.io
```

---

### 2. Grant Permissions

Add both **jenkins** and **ubuntu** users to Docker group:

```bash
sudo su -
usermod -aG docker jenkins
usermod -aG docker ubuntu
```

---

### 3. Restart Docker

```bash
systemctl restart docker
```

---

### 4. Restart Jenkins

After permission changes, restart Jenkins:

```
http://<ec2-instance-public-ip>:8080/restart
```

---

### 5. Verification

* Jenkins can now execute Docker commands
* Docker-based pipelines (agents) will work
* No need for sudo inside pipelines

---

### 6. Final Outcome

✅ Docker installed and configured
✅ Jenkins integrated with Docker daemon
✅ Ready for:

* CI/CD pipelines
* Docker builds
* Containerized deployments

---

## 🚀 What’s Next?

You are now ready to:

* Create Jenkins Pipelines
* Build and manage Docker images
* Push images to container registries
* Deploy applications to Kubernetes (K8s)

---
