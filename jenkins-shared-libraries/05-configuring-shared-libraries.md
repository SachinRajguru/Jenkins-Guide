
## Section 5: Configuring Jenkins Shared Libraries

### **Connecting Jenkins to Your Shared Library Repository**

> *"Creating a Shared Library is only half the job. Jenkins must know where that library exists before it can use it."*

## Overview

In the previous section, we created our first Jenkins Shared Library by adding a reusable Groovy script inside the `vars` directory.

However, Jenkins still doesn't know that this library exists.

Before any Jenkins Pipeline can use a Shared Library, Jenkins must be configured to:

* Know the library's name
* Know where its Git repository is located
* Know which branch to download
* Know how to load it during pipeline execution

In this section, we will explore the complete configuration process for integrating Jenkins with a Shared Library repository.

## Objectives

By the end of this section, we will be able to:

* Configure a Global Shared Library in Jenkins.
* Understand the purpose of Global Pipeline Libraries.
* Connect Jenkins to a Git repository.
* Configure the default branch.
* Understand Modern SCM.
* Import Shared Libraries into Jenkins Pipelines.
* Understand the `@Library` annotation.
* Understand the `_` (underscore) syntax.
* Execute a Shared Library from a Jenkins Pipeline.

## Before Configuration

Let's understand what happens before configuring Jenkins.

Suppose your Shared Library exists on GitHub.

```text
GitHub

└── jenkins-shared-library

      └── vars

             └── helloWorld.groovy
```

Jenkins has no idea that this repository exists.

If your pipeline calls:

```groovy
helloWorld()
```

Jenkins cannot locate the file.

Result?

Pipeline Failure.

## Why Does Jenkins Need Configuration?

Think of Jenkins as a librarian.

A person asks:

> "Please bring me the book called *DevOps Handbook*."

The librarian replies:

> "Which library should I search?"

Without knowing the library's location, the book cannot be found.

The same applies to Jenkins.

When you call:

```groovy
helloWorld()
```

Jenkins first needs to know:

* Which repository contains the library?
* Which branch should it use?
* Which version should it download?

This information is provided through the **Global Pipeline Library configuration**.

### Step 1 – Open Jenkins Dashboard

Navigate to:

```text
Dashboard

    ↓

Manage Jenkins
```

### Step 2 – Configure System

Inside **Manage Jenkins**, select:

```text
Manage Jenkins

    ↓

System
```

(or **Configure System**, depending on the Jenkins version.)

Scroll until you find:

```text
Global Pipeline Libraries
```

This is where Jenkins Shared Libraries are registered.

### What Is a Global Pipeline Library?

A **Global Pipeline Library** is a Shared Library that can be accessed by any Jenkins Pipeline within the Jenkins instance.

Instead of configuring the library separately for every project, you configure it once globally.

Every pipeline can then reuse it.

#### Visual Representation

```text
                    Jenkins

                       │

           Global Pipeline Library

                       │

           Shared Library Repository

                       │

          ┌────────────┬─────────────┐

          ▼            ▼             ▼

      Pipeline A   Pipeline B    Pipeline C
```

One configuration.

Unlimited pipelines.

### Step 3 – Add a Library

Click:

```text
Add

    ↓

Global Pipeline Library
```

You will see several configuration options.

#### Configuration Option 1 – Library Name

Example:

```text
my-shared-library
```

This name is important.

It is the name that pipelines will later import.

For example:

```groovy
@Library('my-shared-library')
```

The names **must match exactly**.

#### Configuration Option 2 – Default Version

Specifying the branch explicitly.

Example:

```text
main
```

or

```text
master
```

depending on your Git repository.

#### Why Specify a Branch?

A Git repository may contain multiple branches.

```text
Repository

│

├── main

├── development

├── testing

└── release
```

Jenkins needs to know which branch should be used when loading the library.

In most cases, organizations configure the stable branch (such as `main`) as the default version.

#### Configuration Option 3 – Retrieval Method

Select:

```text
Modern SCM
```

#### What Is Modern SCM?

**SCM** stands for **Source Code Management**.

Examples include:

* Git
* GitHub
* GitLab
* Bitbucket

Modern SCM allows Jenkins to retrieve Shared Libraries directly from a version-controlled repository.

#### Configuration Option 4 – Git Repository

Provide the Git repository URL.

Example:

```text
https://github.com/company/jenkins-shared-library.git
```

This tells Jenkins where the Shared Library is stored.

> **Note:** If the repository is private, you'll also need to configure appropriate Git credentials in Jenkins so it can authenticate and clone the repository.

### Save the Configuration

After entering the required details:

* Library Name
* Default Branch
* Repository URL

Click:

```text
Save
```

Your Shared Library is now registered with Jenkins.

### What Happens Internally?

After saving:

```text
Jenkins

    ↓

Knows Library Name

    ↓

Knows Git Repository

    ↓

Knows Branch

↓

Can Download Library

    ↓

Ready for Pipeline Execution
```

The configuration only needs to be done **once** for a given Shared Library.

### Importing the Shared Library

Now that Jenkins knows about the library,

the pipeline must import it.

At the very beginning of the Jenkinsfile, add:

```groovy
@Library('my-shared-library') _
```

This statement tells Jenkins:

> "Load the Shared Library named `my-shared-library` before executing the pipeline."

### Understanding the `@Library` Annotation

The annotation:

```groovy
@Library('my-shared-library')
```

identifies **which Shared Library** should be loaded.

The name must exactly match the name configured under **Global Pipeline Libraries**.

Example:

Configured in Jenkins:

```text
Library Name

    ↓

my-shared-library
```

Pipeline:

```groovy
@Library('my-shared-library')
```

If the names differ, Jenkins will not find the library.

### Understanding the Underscore (`_`)

Many beginners are confused by this syntax:

```groovy
@Library('my-shared-library') _
```

The underscore (`_`) is part of the Groovy syntax used with the `@Library` annotation.

It indicates that the library should be loaded and made available to the pipeline.

You can think of it as completing the annotation statement.

> **Note:** In practice, you'll almost always see the `@Library('library-name') _` syntax used exactly as shown in Jenkins examples and documentation.

## How Jenkins Loads the Library

When the pipeline starts, Jenkins performs the following steps:

```text
Pipeline Starts

    ↓

Read @Library Annotation

    ↓

Locate Global Pipeline Library

    ↓

Connect to Git Repository

    ↓

Download Shared Library

    ↓

Load vars/

    ↓

Make Functions Available

    ↓

Execute Pipeline
```

This process happens automatically before the pipeline stages are executed.

## Calling a Shared Library Function

Assume the `vars` directory contains:

```text
vars/

└── helloWorld.groovy
```

with the following code:

```groovy
def call() {
    echo "Hi from DevOps Team"
}
```

Your Jenkinsfile becomes:

```groovy
@Library('my-shared-library') _

pipeline {

    agent any

    stages {

        stage('Greeting') {

            steps {

                helloWorld()

            }

        }

    }

}
```

Notice that you call the **file name** (`helloWorld`) rather than the `call()` method.

## Why Do We Call the File Name Instead of `call()`?

This is a common point of confusion.

Remember:

```text
helloWorld.groovy

    ↓

Contains

    ↓

def call()
```

When Jenkins encounters:

```groovy
helloWorld()
```

it automatically:

```text
Finds helloWorld.groovy

    ↓

Invokes call()

    ↓

Executes the code
```

The `call()` method is implicit; you never invoke it directly from the Jenkinsfile.

## End-to-End Flow

```text
Developer Commits Code

        │

        ▼

Jenkins Pipeline Starts

        │

        ▼

@Library('my-shared-library') _

        │

        ▼

Jenkins Downloads Shared Library

        │

        ▼

helloWorld()

        │

        ▼

helloWorld.groovy

        │

        ▼

call()

        │

        ▼

echo "Hi from DevOps Team"
```

## Real-World Example

Imagine your organization has a reusable function called:

```groovy
dockerBuild()
```

Instead of embedding Docker build commands in every Jenkinsfile, the Shared Library encapsulates that logic.

Every application simply calls:

```groovy
dockerBuild()
```

If your Docker build process changes—perhaps to include additional build arguments or security checks—you update the Shared Library once, and every pipeline automatically benefits from the improvement.

## Best Practices

✔ Give your Shared Library a meaningful name.

✔ Store it in a dedicated Git repository.

✔ Use a stable branch (such as `main`) as the default version.

✔ Keep the library under version control.

✔ Test Shared Library changes before using them in production pipelines.

✔ Document available functions so other engineers know how to use them.

## Common Mistakes

❌ Forgetting to configure the Global Pipeline Library in Jenkins.

❌ Using a different library name in the Jenkinsfile than the one configured in Jenkins.

❌ Pointing Jenkins to the wrong Git repository or branch.

❌ Omitting the `@Library` annotation.

❌ Calling `call()` directly instead of using the Groovy file name.

## Hands-on Lab

### Objective

Configure Jenkins to use your Shared Library.

### Tasks

1. Open **Manage Jenkins**.
2. Navigate to **System** (or **Configure System**, depending on your Jenkins version).
3. Locate **Global Pipeline Libraries**.
4. Add a new Shared Library.
5. Configure:

   * Library Name
   * Default Branch
   * Modern SCM
   * Git Repository URL
6. Save the configuration.
7. Import the library into a Jenkinsfile using:

   ```groovy
   @Library('my-shared-library') _
   ```
8. Call the `helloWorld()` function and verify the pipeline output.

## Summary

In this section, we explored how to connect Jenkins with your Shared Library repository.

Specifically, we explored:

* What a Global Pipeline Library is.
* How to configure a Shared Library in Jenkins.
* The purpose of the library name, default branch, and Modern SCM.
* How the `@Library` annotation works.
* Why the underscore (`_`) is used.
* How Jenkins automatically loads functions from the `vars` directory.
* Why you call the file name rather than the `call()` method.

With Jenkins now configured to use your Shared Library, the next step is to replace repetitive pipeline stages with reusable library functions, creating cleaner, more maintainable, and standardized Jenkins pipelines.

## Interview Questions & Answers

### 1. What is a Global Pipeline Library in Jenkins?

**Answer:**
A Global Pipeline Library is a Shared Library configured at the Jenkins controller level, making reusable pipeline code available to all Jenkins pipelines within that Jenkins instance.

### 2. Why is the `@Library` annotation used?

**Answer:**
The `@Library` annotation tells Jenkins which Shared Library should be loaded before the pipeline executes. It links the Jenkinsfile to the Shared Library configured under **Global Pipeline Libraries**.

### 3. Why is the underscore (`_`) used after the `@Library` annotation?

**Answer:**
The underscore is part of the Groovy syntax used with the `@Library` annotation. It completes the annotation statement and indicates that the specified Shared Library should be loaded and made available to the pipeline.

### 4. Why do we call `helloWorld()` instead of `call()`?

**Answer:**
Jenkins automatically associates the Groovy file name in the `vars` directory with its `call()` method. When `helloWorld()` is invoked, Jenkins locates `helloWorld.groovy` and executes its `call()` method automatically, so the `call()` method is never invoked directly from the Jenkinsfile.
