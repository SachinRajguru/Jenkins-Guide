
## Section 4: Live Demo – Creating Your First Jenkins Shared Library

### Building Your First Reusable Pipeline Component

> *"Theory explains why Shared Libraries are important. Practice teaches you how to use them."*

## Overview

In the previous sections, we understood:

* What Jenkins Shared Libraries are.
* Why they are used.
* Their advantages.
* How they solve real-world enterprise problems.

Now it's time to build one.

In this section, we will explore how to:

* Create a Shared Library repository.
* Understand the Shared Library folder structure.
* Write our first reusable Groovy library.
* Learn the purpose of the `vars` directory.
* Understand the `def call()` method.
* Follow Jenkins naming conventions.
* Execute a simple reusable function.

By the end of this section, we'll have created our first Jenkins Shared Library.

## Objectives

After completing this section, we will be able to:

* Understand the structure of a Jenkins Shared Library.
* Create a Shared Library repository.
* Explain the purpose of the `vars` directory.
* Write a reusable Groovy script.
* Understand the `def call()` method.
* Follow Shared Library naming conventions.
* Prepare the library for Jenkins integration.

## Before Creating a Shared Library

A Shared Library is simply **another Git repository** containing reusable Groovy scripts.

Unlike a normal application repository, it does **not** contain source code such as Java, Python, or Node.js applications.

Instead, it contains reusable pipeline logic.

### Typical Repository Structure

```text
jenkins-shared-library/
│
├── vars/
│
├── src/
│
├── resources/
│
└── README.md
```

A Shared Library is organized into **three important folders**:

* `vars`
* `src`
* `resources`

However, in this lesson, only the **`vars`** directory is covered because it is sufficient for building most reusable pipeline stages.

The `src` and `resources` directories are more advanced topics and are typically explored later.

## Understanding the Three Directories

### 1. vars/

This is the **most commonly used directory**.

It stores reusable pipeline steps that can be directly called from a Jenkinsfile.

Example:

```text
vars/

├── helloWorld.groovy
│
├── mavenBuild.groovy
│
├── dockerBuild.groovy
│
├── sonarScan.groovy
│
└── deployApplication.groovy
```

Every `.groovy` file represents a reusable pipeline function.

### 2. src/

The `src` directory is used for writing complete Groovy classes.

It is generally used when Shared Libraries become more complex and require object-oriented programming.

Example:

```text
src/

└── com/company/utils/

        BuildUtils.groovy
```

This is an advanced topic and is not required when exploring basic Shared Libraries.

### 3. resources/

This directory stores static files used by Shared Libraries.

Examples include:

* YAML files
* JSON files
* Templates
* Configuration files

Example:

```text
resources/

├── deployment.yaml
│
├── nginx.conf
│
└── email-template.html
```

Again, this directory is beyond the scope of this introductory lesson.

## Why Does the Instructor Focus Only on `vars`?

The `vars` directory is enough for most CI/CD pipelines.

Typical reusable stages such as:

* Checkout
* Build
* SonarQube Scan
* Docker Build
* Docker Push
* Kubernetes Deployment

can all be implemented using files inside `vars`.

Therefore, beginners should first become comfortable with this directory before exploring advanced Shared Library concepts.

## Creating the Shared Library Repository

The first step is to create a dedicated Git repository for storing and managing the Jenkins Shared Library.

For example:

```text
GitHub

    ↓

jenkins-shared-library
```

Inside that repository:

```text
jenkins-shared-library/

└── vars/
```

This is where every reusable pipeline component will reside.

## Creating Your First Shared Library

Suppose we want to reuse a simple greeting message.

Instead of writing:

```groovy
echo "Hi from DevOps Team"
```

inside every Jenkins Pipeline,

we'll create a reusable library.

#### Step 1 – Create the vars Directory

```text
jenkins-shared-library/

└── vars/
```

#### Step 2 – Create a Groovy File

Inside `vars`, create:

```text
helloWorld.groovy
```

Notice the naming style.

## Naming Convention

As a best practice, Shared Library files should follow the **camelCase** naming convention.

Good examples:

```text
helloWorld.groovy

mavenBuild.groovy

dockerBuild.groovy

checkoutCode.groovy

sonarScan.groovy
```

Avoid:

```text
HELLO_WORLD.groovy

HelloWorld.groovy

hello_world.groovy
```

Using meaningful camelCase names improves readability and aligns with common Groovy and Jenkins conventions.

## Writing the Shared Library

Create the file:

```text
vars/

└── helloWorld.groovy
```

Content:

```groovy
def call() {

    echo "Hi from DevOps Team"

    echo "This is a Shared Library example."

}
```

## Understanding the Code

Let's examine it line by line.

### Line 1

```groovy
def call()
```

### What does `def` mean?

In Groovy,

`def` is used to declare a method or variable.

It is similar to writing:

```java
public void method()
```

in Java.

### Why is the Method Named `call()`?

This is one of the most important concepts.

When a Groovy file inside `vars` contains a method named:

```groovy
call()
```

Jenkins automatically executes it whenever the file name is referenced.

You do **not** call the function explicitly.

Instead, Jenkins invokes `call()` behind the scenes.

## Internal Working

Suppose the file is:

```text
helloWorld.groovy
```

Inside:

```groovy
def call() {

    echo "Hello"

}
```

When Jenkins sees:

```groovy
helloWorld()
```

it actually performs:

```text
helloWorld()

    ↓

Open helloWorld.groovy

    ↓

Execute call()

    ↓

Print Hello
```

The file name becomes the function name.

## Why Doesn't Jenkins Need Another Function Name?

Because `call()` acts as the default entry point.

Think of it like:

Java

```java
public static void main()
```

or

Python

```python
if __name__ == "__main__":
```

Jenkins expects the execution to begin with `call()`.

## Understanding the Flow

```text
Jenkinsfile

    ↓

helloWorld()

    ↓

helloWorld.groovy

    ↓

call()

    ↓

echo "Hi from DevOps Team"

    ↓

Pipeline Output
```

This automatic mapping is what makes Shared Libraries so clean and intuitive.

## Real-World Example

Imagine your organization wants every pipeline to display:

```text
--------------------------------

Company CI/CD Pipeline

Powered by DevOps Platform Team

--------------------------------
```

Instead of copying this into hundreds of Jenkinsfiles:

Create:

```text
banner.groovy
```

```groovy
def call() {

    echo "--------------------------------"

    echo "Company CI/CD Pipeline"

    echo "Powered by DevOps Platform Team"

    echo "--------------------------------"

}
```

Now every Jenkinsfile simply calls:

```groovy
banner()
```

If the banner changes in the future, you update only one file.

## Analogy

Think of the `vars` folder as a toolbox.

```text
Toolbox

│

├── Hammer

├── Screwdriver

├── Wrench

├── Pliers
```

Whenever you need a tool,

you simply pick it up.

Similarly,

the `vars` directory contains reusable pipeline "tools."

```text
vars/

│

├── checkoutCode

├── mavenBuild

├── dockerBuild

├── deployApplication
```

Each file performs one reusable task.

## Best Practices

✔ Keep each library focused on a single responsibility.

✔ Use meaningful names.

✔ Follow camelCase naming conventions.

✔ Add comments where necessary.

✔ Store the library in Git for version control.

✔ Document each reusable function in the repository.

## Common Mistakes

❌ Placing Shared Libraries outside the `vars` directory.

❌ Using confusing file names.

❌ Forgetting to define the `call()` method.

❌ Writing multiple unrelated tasks in a single library.

❌ Mixing application-specific logic with reusable code.

## Hands-on Lab

### Objective

Create your first Shared Library.

### Tasks

1. Create a Git repository named `jenkins-shared-library`.
2. Create the `vars` directory.
3. Add `helloWorld.groovy`.
4. Implement the `call()` method.
5. Print two messages using `echo`.
6. Commit and push the repository to GitHub.

## Summary

In this section, we explored how to create the foundation of a Jenkins Shared Library.

You explored:

* The Shared Library repository structure.
* The purpose of the `vars`, `src`, and `resources` directories.
* Why beginners should start with `vars`.
* How to create your first reusable Groovy script.
* The importance of the `def call()` method.
* Recommended naming conventions for Shared Library files.

At this point, your Shared Library exists in Git, but Jenkins is not yet aware of it.

In the next section, you'll configure **Global Pipeline Libraries** in Jenkins, connect the Git repository, understand the `@Library` annotation, and explore how Jenkins loads and executes your Shared Library automatically.

## Interview Questions & Answers

### 1. What are the three main directories in a Jenkins Shared Library?

**Answer:**

* `vars` – Stores reusable pipeline steps that can be called directly from Jenkinsfiles.
* `src` – Contains reusable Groovy classes and packages for advanced logic.
* `resources` – Stores static resources such as YAML, JSON, templates, or configuration files.

### 2. Why is the `vars` directory commonly used?

**Answer:**
The `vars` directory allows developers to create simple, reusable pipeline functions that Jenkins can invoke directly. It is ideal for common CI/CD tasks such as building applications, scanning code, or deploying services, making it the most frequently used directory in Shared Libraries.

### 3. What is the purpose of the `def call()` method?

**Answer:**
The `call()` method serves as the default entry point for a Shared Library script in the `vars` directory. When the library file is referenced from a Jenkinsfile, Jenkins automatically executes the `call()` method without requiring an explicit function name.

### 4. Why is camelCase recommended for Shared Library file names?

**Answer:**
CamelCase improves readability, follows common Groovy and Jenkins naming conventions, and makes Shared Library functions easier to understand and maintain across teams.
