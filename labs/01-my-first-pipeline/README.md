
## 📄 `01-my-first-pipeline`

## Jenkins Docker Agent Verification Pipeline

## Table of Contents

1. [Introduction](#1-introduction)
2. [Objective](#2-objective)
   - [Verification Command](#verification-command)
   - [Success Criteria](#success-criteria)
3. [Lab Architecture](#3-lab-architecture)
4. [Why This Lab Matters](#4-why-this-lab-matters)
5. [Prerequisites](#5-prerequisites)
   - [Infrastructure](#infrastructure)
   - [Jenkins Plugins](#jenkins-plugins)
   - [Permissions Setup](#permissions-setup)
   - [Validation](#validation)
6. [Repository Structure](#6-repository-structure)
7. [Jenkinsfile](#7-jenkinsfile)
8. [Pipeline Explanation](#8-pipeline-explanation)
   - [Agent (Docker)](#agent-docker)
   - [Stage: Validation](#stage-validation)
9. [Execution Flow](#9-execution-flow)
10. [Validation](#10-validation)
11. [Key Takeaways](#11-key-takeaways)
12. [Next Lab](#12-next-lab)

## 1. Introduction

This lab validates a **Jenkins pipeline running inside a Docker-based agent**.

It confirms the foundational CI setup:

* Jenkins is installed and operational
* Docker integration with Jenkins is working
* Jenkins can provision ephemeral container agents
* Pipeline execution works inside containers

> This is a foundational infrastructure validation pipeline, not a CI/CD workflow.

## 2. Objective

To execute a Jenkins pipeline inside a Docker container and verify the build environment.

### Verification Command

```bash
node --version
```

### Success Criteria

* Docker container is provisioned successfully
* Node.js runtime is available inside container
* Jenkins pipeline executes without agent errors
* Container is destroyed after execution

## 3. Lab Architecture

```text
Developer
   ↓
Jenkins Pipeline Trigger
   ↓
Docker Agent Requested
   ↓
node:20-alpine Container Created
   ↓
Pipeline Executes Inside Container
   ↓
Node Version Printed
   ↓
Container Destroyed (Ephemeral Agent)
```

## 4. Why This Lab Matters

This lab validates the CI foundation:

* Docker plugin integration in Jenkins
* Ephemeral container-based execution
* Secure and isolated build environments
* Reproducible CI execution model

## 5. Prerequisites

### Infrastructure

* Jenkins server (VM / EC2 / local)
* Docker installed and running
* Jenkins accessible on port `8080`

### Jenkins Plugins

* Docker Pipeline Plugin
* Git Plugin

### Permissions Setup

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart docker
sudo systemctl restart jenkins
```

### Validation

```bash
sudo su - jenkins
docker run hello-world
```

## 6. Repository Structure

```text
01-my-first-pipeline/
├── Jenkinsfile
└── README.md
```

## 7. Jenkinsfile

```groovy
pipeline {
    agent {
        docker {
            image 'node:20-alpine'
        }
    }

    stages {
        stage('Validation') {
            steps {
                sh 'node --version'
            }
        }
    }
}
```

## 8. Pipeline Explanation

### Agent (Docker)

The pipeline runs inside:

```text
node:20-alpine
```

Why this image:

* Lightweight Alpine Linux
* Pre-installed Node.js 20
* Fast startup time
* Clean and reproducible CI environment

### Stage: Validation

Confirms runtime availability:

```bash
node --version
```

## 9. Execution Flow

```text
Trigger Build
→ Pull Docker Image
→ Create Container
→ Mount Workspace
→ Execute Command
→ Print Node Version
→ Destroy Container
```

This is known as:

> **Ephemeral CI Execution Model**

## 10. Validation

Check running containers:

```bash
docker ps
```

Expected:

* No active containers after build

Check history:

```bash
docker ps -a
```

## 11. Key Takeaways

* Jenkins successfully runs Docker-based agents
* CI execution is ephemeral and isolated
* Environment is reproducible across runs
* Foundation for advanced pipelines is validated

## 12. Next Lab

```text
02-multi-stage-multi-agent/
```

You will extend into:

* Multi-stage pipelines
* Multiple runtime environments
* Stage-level Docker agents
* Real CI/CD architecture simulation
