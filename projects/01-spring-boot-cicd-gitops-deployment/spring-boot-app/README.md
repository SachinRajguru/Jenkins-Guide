# Spring Boot Application

## Build & Execution Guide

## Table of Contents

- [1. Overview](#1-overview)
- [2. Technology Stack](#2-technology-stack)
- [3. Project Structure](#3-project-structure)
- [4. Prerequisites](#4-prerequisites)
- [5. Navigate the Application Directory](#5-navigate-the-application-directory)
- [6. Build the Application](#6-build-the-application)
- [7. Run the Application](#7-run-the-application)
  - [7.1 Run Using Java](#71-run-using-java)
  - [7.2 Run Using Docker](#72-run-using-docker)
    - [Build Docker Image](#build-docker-image)
    - [Run Docker Container](#run-docker-container)
    - [Verify Running Container](#verify-running-container)
    - [Access the Application](#access-the-application)
- [8. Application Verification](#8-application-verification)
- [9. Summary](#9-summary)

## 1. Overview

This project is a simple **Spring Boot MVC web application** built using **Apache Maven**.

The application follows the **Model-View-Controller (MVC)** architectural pattern, where:

* The **Controller** handles incoming HTTP requests.
* The **Model** manages application data.
* The **View** renders the response using templates.

The controller returns a web page populated with model attributes such as **title** and **message**.

This application is used as the sample workload for the **Spring Boot CI/CD & GitOps Deployment** project, where it is packaged, containerized, and deployed through an automated delivery workflow.

## 2. Technology Stack

| Component            | Technology                  |
| -------------------- | --------------------------- |
| Programming Language | Java 11                     |
| Framework            | Spring Boot                 |
| Build Tool           | Apache Maven                |
| Architecture         | MVC (Model-View-Controller) |
| Template Engine      | Thymeleaf                   |
| Containerization     | Docker                      |

## 3. Project Structure

Spring Boot dependencies are managed using the **`pom.xml`** file located in the application root directory.

The Maven build process compiles the source code and packages the application into an executable JAR file.

```text
spring-boot-app/
│
├── src/
│   └── main/
│       ├── java/
│       │   └── com/
│       │       └── sachin/
│       │           └── StartApplication.java
│       │
│       └── resources/
│           ├── templates/
│           │   └── index.html
│           │
│           └── static/
│               ├── css/
│               │   └── main.css
│               │
│               └── js/
│                   └── main.js
│
├── Dockerfile
├── pom.xml
├── README.md
├── .dockerignore
└── .gitignore
```

## 4. Prerequisites

Before building and running the application, ensure the following tools are installed.

| Tool         | Version                   |
| ------------ | ------------------------- |
| Java         | 11                        |
| Apache Maven | 3.8+                      |
| Docker       | Latest version (Optional) |

Verify Java installation:

```bash
java -version
```

Verify Maven installation:

```bash
mvn -version
```

Verify Docker installation:

```bash
docker --version
```

## 5. Navigate to the Application Directory

Navigate to the Spring Boot application directory:

```bash
cd spring-boot-app
```

## 6. Build the Application

Execute the following Maven command:

```bash
mvn clean package
```

### Build Process

The above command performs the following operations:

* Removes previous build artifacts.
* Downloads required Maven dependencies.
* Compiles the application source code.
* Executes configured tests.
* Packages the application into an executable JAR file.

The generated artifact is created inside the `target/` directory.

Example:

```text
target/spring-boot-web.jar
```

## 7. Run the Application

The application can be executed using either:

1. Java runtime
2. Docker container

Running the application using Docker is recommended because it provides a consistent runtime environment.

### 7.1 Run Using Java

Start the application:

```bash
java -jar target/spring-boot-web.jar
```

Once the application starts successfully, access it using:

```text
http://localhost:8080
```

### 7.2 Run Using Docker

#### Build Docker Image

The Docker image is created using the `Dockerfile` available in the application root directory.

Build the image:

```bash
docker build -t spring-boot-app:v1 .
```

Verify the Docker image:

```bash
docker images
```

#### Run Docker Container

Start the application container:

```bash
docker run -d \
  --name springboot-app \
  -p 8010:8080 \
  spring-boot-app:v1
```

#### Verify Running Container

Check running containers:

```bash
docker ps
```

View application logs:

```bash
docker logs springboot-app
```

#### Access the Application

For local execution:

```text
http://localhost:8010
```

For remote server execution:

```text
http://<server-ip>:8010
```

## 8. Application Verification

After starting the application, verify the following:

* Application starts without errors.
* Home page is accessible from the browser.
* Page title is displayed correctly.
* Welcome message is rendered successfully.
* Container logs show successful application startup.

## 9. Summary

In this guide, you learned how to:

* Understand the Spring Boot application structure.
* Build the application using Maven.
* Generate an executable JAR artifact.
* Run the application locally using Java.
* Build and run the application using Docker.
* Verify application availability.

This Spring Boot application acts as the sample workload for the complete CI/CD and GitOps deployment workflow using tools such as Jenkins, SonarQube, Docker, Kubernetes, Helm, and Argo CD.
