
## 📄 `02-multi-stage-multi-agent`

## Jenkins Multi-Stage Multi-Agent Pipeline (Docker-Based CI Model)

## Table of Contents

1. [Introduction](#1-introduction)
2. [Learning Outcomes](#2-learning-outcomes)
3. [Objective](#3-objective)
4. [Multi-Stage Concept](#4-multi-stage-concept)
5. [Multi-Agent Model](#5-multi-agent-model)
6. [Architecture](#6-architecture)
7. [Jenkinsfile](#7-jenkinsfile)
8. [Execution Flow](#8-execution-flow)
9. [Why `agent none`](#9-why-agent-none)
10. [Workspace Sharing](#10-workspace-sharing)
11. [CI Execution Model](#11-ci-execution-model)
12. [Why This Matters](#12-why-this-matters)
13. [Real-World Mapping](#13-real-world-mapping)
14. [Key Takeaways](#14-key-takeaways)
15. [What's Next](#15-whats-next)
16. [Final Insight](#final-insight)

## 1. Introduction

This lab extends the single-stage pipeline into a **multi-stage, multi-agent CI system**.

Each stage runs in a separate Docker container, simulating real-world CI/CD architecture.

## 2. Learning Outcomes

You will learn:

* Multi-stage pipeline design
* Stage-level Docker agents
* `agent none` usage
* Ephemeral CI execution model
* Workspace sharing between stages
* Real CI/CD architecture patterns

## 3. Objective

Simulate a **3-tier CI pipeline** using Docker agents:

* Frontend → Node.js runtime
* Backend → Maven + JDK runtime
* Database → MySQL runtime validation

## 4. Multi-Stage Concept

```text
Frontend → Backend → Database
```

Each stage:

* Runs independently
* Uses a dedicated runtime
* Executes inside isolated containers

## 5. Multi-Agent Model

| Stage    | Docker Image                    |
| -------- | ------------------------------- |
| Frontend | node:20-alpine                  |
| Backend  | maven:3.9.15-eclipse-temurin-17 |
| Database | mysql:latest                    |

## 6. Architecture

```text
Git Push
   ↓
Jenkins Controller
   ↓
 ┌────────────┬────────────┬────────────┐
 │ Frontend   │ Backend    │ Database   │
 │ (Node)     │ (Maven)    │ (MySQL)    │
 └────────────┴────────────┴────────────┘
   ↓            ↓            ↓
Containers created per stage
   ↓
Destroyed after execution
```

## 7. Jenkinsfile

```groovy
pipeline {
    agent none

    stages {

        stage('Frontend') {
            agent {
                docker { image 'node:20-alpine' }
            }
            steps {
                sh 'node --version'
            }
        }

        stage('Backend') {
            agent {
                docker { image 'maven:3.9.15-eclipse-temurin-17' }
            }
            steps {
                sh 'mvn -v'
            }
        }

        stage('Database') {
            agent {
                docker { image 'mysql:latest' }
            }
            steps {
                sh 'mysql --version'
            }
        }
    }
}
```

## 8. Execution Flow

```text
Frontend Stage  → Container A → Destroy
Backend Stage   → Container B → Destroy
Database Stage  → Container C → Destroy
```

Each stage is:

> Ephemeral + Isolated + Independent

## 9. Why `agent none`

* Forces stage-level execution control
* Prevents shared execution environment
* Enables multi-runtime pipelines
* Improves isolation and reproducibility

## 10. Workspace Sharing

* Jenkins shares workspace across containers
* Files persist between stages
* Enables multi-stage CI workflows

## 11. CI Execution Model

```text
Provision → Execute → Collect Logs → Destroy
```

## 12. Why This Matters

Traditional CI:

* Static agents
* Dependency conflicts
* Manual setup

Docker CI:

* Ephemeral environments
* Clean execution per stage
* Scalable architecture
* Production-aligned design

## 13. Real-World Mapping

| Lab Concept     | Industry Equivalent    |
| --------------- | ---------------------- |
| node container  | frontend build runner  |
| maven container | backend CI runner      |
| mysql container | DB migration stage     |
| Jenkins agent   | CI worker node         |
| agent none      | pipeline orchestration |

## 14. Key Takeaways

* Multi-stage CI pipelines
* Multi-agent execution model
* Docker-based isolation
* Ephemeral infrastructure pattern
* Real-world CI/CD architecture simulation

## 15. What's Next

You will move into:

* Real application CI pipeline
* Docker image build
* Container registry push
* Kubernetes deployment
* GitOps with Argo CD

## Final Insight

This architecture represents modern CI/CD systems:

> Each stage runs in a dedicated ephemeral container, orchestrated by Jenkins for isolated and reproducible execution.
