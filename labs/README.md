
## 📄 `labs/README.md`

## Jenkins Labs Overview

## Table of Contents

1. [Introduction](#1-introduction)
2. [What This Contains](#2-what-this-contains)
3. [Learning Path](#3-learning-path)
   - [Lab 1](#lab-1)
   - [Lab 2](#lab-2)
4. [Lab Structure](#4-lab-structure)
5. [How to Run](#5-how-to-run)
6. [Core Concepts](#6-core-concepts)
7. [CI Execution Flow](#7-ci-execution-flow)
8. [Requirements](#8-requirements)
9. [Key Value](#9-key-value)
10. [Next Step](#10-next-step)
11. [Final Note](#final-note)

## 1. Introduction

This repository contains **hands-on Jenkins labs using Docker-based execution environments**.

It takes you from:

> Jenkins basics → Multi-stage CI → Real-world pipeline architecture

## 2. What This Contains

```text
labs/
├── 01-my-first-pipeline
├── 02-multi-stage-multi-agent
└── README.md
```

## 3. Learning Path

### Lab 1

* Jenkins + Docker integration
* First pipeline execution
* Node.js container validation

### Lab 2

* Multi-stage pipelines
* Multi-agent execution model
* Real CI/CD simulation

## 4. Lab Structure

Each lab contains:

```text
Jenkinsfile → Pipeline definition
README.md   → Execution guide
```

## 5. How to Run

1. Clone repo
2. Create Jenkins pipeline job
3. Point to Jenkinsfile path
4. Run build

## 6. Core Concepts

* Jenkins Pipeline as Code
* Docker-based CI execution
* Ephemeral agents
* Multi-stage pipelines
* Multi-runtime environments

## 7. CI Execution Flow

```text
Trigger → Jenkins → Docker Container → Execute → Destroy
```

## 8. Requirements

* Jenkins installed
* Docker installed
* Jenkins Docker plugin
* Jenkins Docker permissions configured

## 9. Key Value

After completing these labs, you understand:

* How real CI pipelines run
* How Docker is used in CI
* How multi-stage pipelines work
* How modern CI/CD systems are designed

## 10. Next Step

Advance to:

* Docker image build pipelines
* Kubernetes deployments
* GitOps with Argo CD
* Production CI/CD systems

## Final Note

These labs simulate **real-world CI/CD engineering practices using Jenkins + Docker**, not simplified tutorials.
