
# 📄 `01-cicd-fundamentals.md`

# CI/CD – Study + Practical + Lab + Technical Guide

---

## Table of Contents

1. [Introduction to CI/CD](#1-introduction-to-cicd)
2. [What is CI/CD?](#what-is-cicd)
3. [CI vs CD (Core Difference)](#ci-vs-cd-core-difference)
4. [Standard CI/CD Pipeline](#standard-cicd-pipeline-steps-lab-guide)
5. [Detailed Pipeline Steps](#detailed-step-by-step-explanation)
6. [Developer Workflow](#developer-workflow)
7. [Legacy CI/CD (Jenkins)](#legacy-cicd-jenkins)
8. [Environment Promotion](#environment-promotion-very-important)
9. [Historical Context of Jenkins](#historical-context-of-jenkins)
10. [Modern CI/CD (Cloud Native)](#modern-cicd-cloud-native)
11. [Tool Ecosystem](#cicd-tool-ecosystem)
12. [Lab Preparation](#lab-preparation-next-step)
13. [Final Summary](#final-summary)

---

## 1. Introduction to CI/CD

CI/CD is a core DevOps practice. It connects:

**Development → Testing → Deployment (Automation)**

### Goals of This Guide

* Understand CI/CD fundamentals
* Learn legacy vs modern CI/CD setups
* Explore tools (Jenkins, GitHub Actions, etc.)
* Understand real-world workflows
* Prepare for hands-on implementation

---

## What is CI/CD?

### Definition

CI/CD stands for:

* **CI → Continuous Integration**
* **CD → Continuous Delivery / Continuous Deployment**

---

### Continuous Integration (CI)

CI is the process of automatically integrating code changes into a shared repository with validation steps.

**Includes:**

* Testing
* Code analysis
* Quality checks

**Ensures:**

* Code is correct, secure, and ready

**Simple Meaning:**
Every time a developer pushes code → system automatically validates it

---

### Continuous Delivery (CD)

CD is the process of automatically preparing and deploying applications.

**Ensures:**

* Reliable delivery
* Availability to users

**Simple Meaning:**
After testing → code is deployed to environments

---

## CI vs CD (Core Difference)

| Phase | Description            | Purpose             |
| ----- | ---------------------- | ------------------- |
| CI    | Build + Test + Scan    | Ensure code quality |
| CD    | Deploy to environments | Deliver to users    |

**Formula:**

* CI = Build + Test + Scan
* CD = Deploy (Dev → Staging → Production)

---

### 🍽️ Analogy

* CI = Chef prepares and checks food
* CD = Food served to customers

---

### 📘 Textbook Summary

* CI automates validation
* CD automates delivery

---

## Real-World Scenario

**Problem:**
Developer builds code locally → Customer is elsewhere

➤ How does code reach customer reliably?

**Answer:** CI/CD Pipeline

---

## Standard CI/CD Pipeline Steps (Lab Guide)

### Pipeline Flow

Developer → VCS → CI/CD Tool → Customer

### Steps

1. Unit Testing
2. Static Code Analysis
3. Code Quality & Security Scan
4. Automation / E2E Testing
5. Reporting
6. Deployment

---

## Detailed Step-by-Step Explanation

### 1. Unit Testing

**Definition:** Testing individual functions

```python
def add(a, b):
    return a + b

def test_add():
    assert add(2, 3) == 5
```

**Why Automation?**

* Frequent changes
* Manual testing impossible

**Interview Q**
**Q:** What is unit testing?
**A:** Testing a single function independently

---

### 2. Static Code Analysis

**Definition:** Analyze code without execution

**Checks:**

* Syntax errors
* Unused variables
* Formatting

**Tools:**

* SonarQube
* ESLint
* Pylint

**Analogy:** Grammar check

---

### 3. Code Quality & Security Testing

**Goal:**

* Secure applications
* Detect vulnerabilities

**Example:**

* Log4Shell vulnerability → Deployment blocked

**Tools:**

* Snyk
* OWASP
* Checkmarx
* Dependabot

---

### 4. Automation / E2E Testing

**Definition:** Test full application flow

**Example Flow:**
Login → Action → Logout

| Type | Scope    |
| ---- | -------- |
| Unit | Function |
| E2E  | Full app |

---

### 5. Reporting

**Tracks:**

* Test coverage
* Failures
* Security status

**Tools:**

* Allure
* ELK Stack

---

### 6. Deployment

**Definition:** Making application accessible

**Targets:**

* VMs
* Containers
* Kubernetes

**Key Insight:**
Without deployment → users cannot access app

---

## Developer Workflow

### Modern Workflow

Small changes → Frequent commits → Automated pipeline

### Flow

1. Write code
2. Push to VCS (GitHub, GitLab, Bitbucket)
3. Pipeline triggers
4. Tests + Deploy run automatically

---

## Legacy CI/CD (Jenkins)

### Architecture

Developer → GitHub → Jenkins → Tools → Deployment

---

### What is Jenkins?

Jenkins is an automation server used to orchestrate CI/CD pipelines.

---

### Core Concept

Jenkins **does NOT execute tasks itself**
It **orchestrates tools**

---

### Pipeline Example

```groovy
pipeline {
    agent any   // Run on any available Jenkins agent

    stages {
        stage('Build') {
            steps {
                // Compile and package the application
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                // Run unit tests
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy to Kubernetes cluster
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
```

---

### Analogy

* Developer = Worker
* Jenkins = Manager
* Tools = Specialists

---

### Interview Highlights

* Jenkins = Orchestrator
* Triggered by Git events
* Uses tools (JUnit, Maven, etc.)

---

## Environment Promotion (VERY IMPORTANT)

### Flow

Dev → Staging → Production

---

### Environments

#### Dev

* Fast, simple
* Frequent deployments

#### Staging

* Production-like
* QA testing

#### Production

* Live users
* High stability

---

### Key Principle

Same code → Promoted across environments

---

### Why Not Direct Production?

* High risk
* Expensive
* Slower

---

## Historical Context of Jenkins

### Timeline

* 2004 → Hudson
* 2011 → Jenkins

---

### Problems with Legacy CI/CD

1. High cost
2. Manual scaling
3. No scale-to-zero
4. Maintenance overhead

---

### Analogy

Always-open office → even with no work

---

## Modern CI/CD (Cloud Native)

### Example Stack

* Kubernetes
* GitHub Actions

---

### GitHub Actions

**Definition:** Event-driven CI/CD platform

---

### Key Concept: Ephemeral Compute

Start → Run → Destroy

---

### Benefits

* No idle cost
* Auto scaling
* Faster execution

---

### Analogy

Like Uber → Pay only when used

---

## Jenkins vs Modern CI/CD

| Feature   | Jenkins        | GitHub Actions |
| --------- | -------------- | -------------- |
| Setup     | Manual         | Built-in       |
| Scaling   | Manual         | Automatic      |
| Cost      | High           | Pay-per-use    |
| Execution | Always running | On-demand      |

---

## CI/CD Tool Ecosystem

### Legacy

* Jenkins

### Modern

* GitHub Actions
* GitLab CI/CD
* CircleCI
* Travis CI

---

## Lab Preparation (Next Step)

You will implement:

* Jenkins setup
* Pipelines
* GitHub Actions
* End-to-end CI/CD

---

## Final Summary

### Key Takeaways

* CI/CD = Automation of delivery
* Pipeline = Build → Test → Deploy
* Jenkins = Orchestrator
* Modern CI/CD = Scalable + Cost-efficient

---

### Final Insight

Industry shift:

**Static Infrastructure (Jenkins)**
⬇
**Dynamic, On-Demand Systems (Cloud-Native CI/CD)**

---