# Java Security Checks

This repository demonstrates how to set up automated security checks using GitHub Actions for a simple Java project. Follow the steps below to set up your own repository and run security scans on your Java code.

## Step 1: Setting Up Your GitHub Repository

1. **Create a GitHub Repository**:
   - Go to [GitHub](https://github.com) and sign in.
   - Click on the plus sign (+) in the top right corner and select "New repository".
   - Name your repository, for example, `java-security-checks`, and click "Create repository".

## Step 2: Adding Your Java Code

1. **Create a Simple Java Project**:
   - In your repository, create a new file named `HelloWorld.java` and add the following simple Java code:

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}

## Step 3: Creating a GitHub Actions Workflow
**1. Create a Workflow File:

In your repository, create a new folder named .github and inside it, another folder named workflows.
Inside the workflows folder, create a file named security-checks.yml.
Setting Up the Workflow File:

Open the security-checks.yml file and add the following content:

name: Security Checks

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  security:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up JDK 11
        uses: actions/setup-java@v2
        with:
          distribution: 'adopt'
          java-version: '11'

      - name: Run Security Scan
        run: |
          curl -LO https://github.com/jeremylong/DependencyCheck/releases/download/v6.5.3/dependency-check-6.5.3-release.zip
          unzip dependency-check-6.5.3-release.zip
          ./dependency-check/bin/dependency-check.sh --project JavaSecurityCheck --scan ./ --format HTML --out report

      - name: Upload Report
        uses: actions/upload-artifact@v2
        with:
          name: security-report
          path: report

## Step 4: Understanding the Workflow
** Workflow Explanation:
- name: Names the workflow "Security Checks".
- on: Specifies the events that trigger the workflow. In this case, it runs when you push code to the main branch or create a pull request targeting the main branch.
- jobs: Defines the tasks to be performed. Here, we have a job named security.
** Job Steps:
- runs-on: Specifies the type of machine to run the job on. We use ubuntu-latest.
- steps: Lists the actions to perform:
 -- actions/checkout@v2: Checks out your code.
 -- actions/setup-java@v2: Sets up Java Development Kit (JDK) version 11.
 -- Run Security Scan: Downloads and runs DependencyCheck, a tool that scans for known vulnerabilities in project dependencies.
 -- actions/upload-artifact@v2: Uploads the generated security report as an artifact.

## Step 5: Committing and Pushing the Workflow
 - Commit the Workflow File:
 - Save the security-checks.yml file.
 - Add, commit, and push this file to your GitHub repository.


## Step 6: Viewing the Results
  ** Check the Workflow Results:
       - Go to your GitHub repository.
       - Click on the "Actions" tab. You will see the workflow running.
       - Once it completes, you can view the results to see if any security issues were found.
## Summary
You have now set up an automated workflow that runs security checks on your Java code every time you push changes or create a pull request. This helps ensure that your code is free from common security vulnerabilities.
