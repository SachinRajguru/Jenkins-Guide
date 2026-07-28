
##  Section 6: Replacing Pipeline Code with Jenkins Shared Libraries

### **Transforming Long Jenkinsfiles into Clean, Reusable Pipelines**

> *"A good Jenkins Pipeline doesn't contain every implementation detail. Instead, it orchestrates reusable building blocks."*

## Overview

In the previous section, we configured Jenkins to recognize our Shared Library.

Now comes the exciting part—**actually using it.**

Instead of writing dozens (or even hundreds) of lines inside every Jenkinsfile, we'll replace repetitive code with simple Shared Library function calls.

In this section, we will explore how a simple "Hello World" example is gradually transformed into a reusable library. We'll then extend the same idea to real-world CI/CD stages such as Maven builds.

By the end of this section, we'll understand how Shared Libraries dramatically simplify Jenkins Pipelines while improving readability, maintainability, and scalability.

## Objectives

By the end of this section, we will be able to:

* Replace repetitive pipeline stages with Shared Libraries.
* Understand how Jenkins executes Shared Library functions.
* Convert a simple stage into a reusable library.
* Create a reusable Maven build library.
* Understand how Shared Libraries standardize enterprise pipelines.
* Design cleaner and more maintainable Jenkinsfiles.

## Step 1 – Start with a Normal Jenkins Pipeline

We begin with a simple pipeline to verify that Jenkins is working correctly.

```groovy
pipeline {

    agent any

    stages {

        stage('Greeting') {

            steps {

                echo "Hello World"

            }

        }

    }

}
```

### Expected Output

```text
Hello World
```

At this point, there is nothing related to Shared Libraries.

The goal is simply to confirm that Jenkins is functioning correctly.

## Why Start with a Simple Pipeline?

Before introducing Shared Libraries, it's important to verify that:

* Jenkins is installed correctly.
* The pipeline executes successfully.
* The Jenkins agent is functioning properly.

If a basic pipeline fails, the issue is unrelated to Shared Libraries and should be resolved first.

> **Best Practice:** Always validate your Jenkins environment with a simple pipeline before introducing additional complexity.

## Step 2 – Identify Repetitive Code

Now imagine every Jenkins Pipeline begins with the following stage:

```groovy
stage('Greeting') {

    steps {

        echo "Hi from DevOps Team"

    }

}
```

Suppose your organization has **200 pipelines**, and every one of them displays this greeting.

Instead of copying this block into every Jenkinsfile, we can move it into a Shared Library.

## Step 3 – Create a Shared Library

Create the following file:

```text
vars/

└── helloWorld.groovy
```

Contents:

```groovy
def call() {

    echo "Hi from DevOps Team"

    echo "This is a Shared Library example."

}
```

This file now contains the reusable implementation.

## Step 4 – Import the Shared Library

At the top of the Jenkinsfile:

```groovy
@Library('my-shared-library') _
```

This loads the Shared Library before the pipeline begins.

## Step 5 – Replace the Original Code

Originally:

```groovy
stage('Greeting') {

    steps {

        echo "Hi from DevOps Team"

        echo "This is a Shared Library example."

    }

}
```

After introducing the Shared Library:

```groovy
stage('Greeting') {

    steps {

        helloWorld()

    }

}
```

Notice how the pipeline becomes significantly shorter.

## What Happens Behind the Scenes?

When Jenkins encounters:

```groovy
helloWorld()
```

it performs the following sequence:

```text
helloWorld()

    │

    ▼

Locate helloWorld.groovy

    │

    ▼

Execute call()

    │

    ▼

Display Messages
```

The pipeline doesn't know how the function works internally.

It simply requests Jenkins to execute it.

This is known as **abstraction**.

## Understanding Abstraction

### Definition

**Abstraction** is the process of hiding implementation details while exposing only the functionality required by the user.

#### Everyday Example

Think about driving a car.

You press:

* Accelerator
* Brake
* Steering Wheel

You don't need to know:

* Fuel injection timing
* Engine combustion
* Gear synchronization

The complex implementation is hidden.

Similarly,

```groovy
mavenBuild()
```

hides all the Maven commands.

## Pipeline Before and After

### Before Shared Libraries

```text
Pipeline

    ↓

Checkout

    ↓

Build

    ↓

Test

    ↓

Docker Build

    ↓

Push Image

    ↓

Deploy
```

Hundreds of lines.

### After Shared Libraries

```text
Pipeline

    ↓

checkoutCode()

    ↓

mavenBuild()

    ↓

dockerBuild()

    ↓

dockerPush()

    ↓

deployApplication()
```

Much cleaner.

Much easier to understand.

## Extending the Concept to Maven

The "Hello World" is only a simple demonstration.

The real value comes from replacing actual CI/CD stages.

Suppose your pipeline contains:

```groovy
stage('Build') {

    steps {

        sh 'mvn clean install'

    }

}
```

This stage exists in almost every Java-based pipeline.

## Without Shared Libraries

Every Jenkinsfile contains:

```groovy
stage('Build') {

    steps {

        sh 'mvn clean install'

    }

}
```

Repeated hundreds of times.

## Creating a Maven Shared Library

Inside:

```text
vars/

└── mavenBuild.groovy
```

Write:

```groovy
def call() {

    sh 'mvn clean install'

}
```

That's it.

## Using the Library

Instead of writing:

```groovy
stage('Build') {

    steps {

        sh 'mvn clean install'

    }

}
```

simply write:

```groovy
stage('Build') {

    steps {

        mavenBuild()

    }

}
```

The pipeline now delegates the implementation to the Shared Library.

## Internal Execution Flow

```text
Jenkinsfile

    ↓

mavenBuild()

    ↓

vars/mavenBuild.groovy

    ↓

call()

    ↓

mvn clean install
```

Everything is executed automatically.

## Why Is This Better?

Suppose tomorrow the build command changes to:

```bash
mvn clean verify
```

#### Without Shared Libraries:

Modify every Jenkinsfile.

#### With Shared Libraries:

Modify:

```text
vars/

└── mavenBuild.groovy
```

Only one file changes.

Every pipeline benefits.

## Standardizing Pipelines

Shared Libraries allow organizations to define a standard pipeline.

Example:

```groovy
@Library('my-shared-library') _

pipeline {

    agent any

    stages {

        stage('Checkout') {

            steps {

                checkoutCode()

            }

        }

        stage('Build') {

            steps {

                mavenBuild()

            }

        }

        stage('Code Quality') {

            steps {

                sonarScan()

            }

        }

        stage('Docker') {

            steps {

                dockerBuild()

            }

        }

        stage('Deploy') {

            steps {

                deployApplication()

            }

        }

    }

}
```

Notice how readable the pipeline becomes.

Anyone—even a new team member—can understand its purpose immediately.

## Enterprise Scenario

Imagine your company uses the following standard build process:

```text
Checkout

    ↓

Maven Build

    ↓

SonarQube

    ↓

Docker

    ↓

Security Scan

    ↓

Deploy
```

Every Java application follows this workflow.

Instead of rewriting these stages for each project, the platform engineering team creates reusable Shared Libraries.

Now, onboarding a new project is as simple as creating a short Jenkinsfile that references these standardized functions.

This ensures:

* Consistency
* Faster onboarding
* Easier maintenance
* Reduced errors

## Best Practices

✔ Create one Shared Library for one responsibility.

✔ Use descriptive names such as:

* `checkoutCode()`
* `mavenBuild()`
* `dockerBuild()`
* `sonarScan()`
* `deployApplication()`

✔ Keep implementation details inside the library.

✔ Keep Jenkinsfiles focused on orchestration rather than implementation.

✔ Document every reusable function.

## Common Mistakes

❌ Creating one huge Shared Library that performs multiple unrelated tasks.

❌ Writing project-specific code inside reusable libraries.

❌ Duplicating code that should reside in Shared Libraries.

❌ Giving Shared Libraries unclear or inconsistent names.

❌ Treating Shared Libraries as shortcuts instead of standardized building blocks.

## Hands-on Lab

### Objective

Convert an existing Jenkins Pipeline into one that uses Shared Libraries.

### Tasks

1. Create `mavenBuild.groovy` in the `vars` directory.
2. Move the `mvn clean install` command into the `call()` method.
3. Commit and push the updated Shared Library.
4. Import the library into your Jenkinsfile.
5. Replace the original build stage with `mavenBuild()`.
6. Execute the pipeline and verify that the build completes successfully.

## Summary

In this section, we explored how to replace repetitive Jenkins Pipeline code with reusable Shared Library functions.

You explored:

* Converting a simple "Hello World" example into a Shared Library.
* Understanding abstraction in Jenkins Pipelines.
* Creating a reusable Maven build library.
* Replacing verbose build stages with concise function calls.
* Standardizing enterprise CI/CD pipelines using reusable components.

At this point, your Jenkinsfile has become significantly cleaner and easier to maintain. Rather than containing detailed implementation logic, it now acts as an orchestrator that invokes reusable Shared Library functions.

## Interview Questions & Answers

### 1. Why should repetitive pipeline stages be moved into Shared Libraries?

**Answer:**
Repetitive stages increase code duplication and maintenance effort. By moving them into Shared Libraries, organizations centralize common logic, improve consistency, simplify updates, and make Jenkinsfiles shorter and easier to maintain.

### 2. What is the advantage of replacing `sh 'mvn clean install'` with `mavenBuild()`?

**Answer:**
The actual Maven command is centralized inside the Shared Library. If the build process changes in the future, only the Shared Library needs to be updated, and every pipeline automatically uses the updated implementation.

### 3. What software engineering principle does this approach demonstrate?

**Answer:**
It demonstrates **abstraction** and **code reuse**. The Jenkinsfile focuses on *what* should happen (e.g., `mavenBuild()`), while the Shared Library contains *how* it happens.

### 4. Why do Shared Libraries make Jenkinsfiles easier to read?

**Answer:**
Shared Libraries replace detailed implementation code with descriptive function calls. This results in shorter, cleaner Jenkinsfiles that are easier to understand, review, and maintain, especially in large enterprise environments.
