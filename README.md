# Infrastructure Automation CI/CD Lab

## Introduction:

In this lab, you will learn use a pipeline tool, GitHub Actions, to build a simple pipeline job.

## Objective

For this lab, you will gain hands-on experience using GitHub Actions which uses a provisioned container to execute jobs when a specific "action" takes place inside a GitHub repository.

## Getting Started:

1. Copy the starter code from here into a new, private repository on your GitHub account. When adding a collaborator, be sure to add the instructor ("CSCC-Instructor").
2. Add a new file to the directory called GitHubActionsQuestions.txt. You will update this file with answers the questions stated throughout the lab.

## Setting up the Starter Code

This repository will be intially empty other than the README.md with these instructions you are reading. 
We will initialize a new Gradle project then Create a GitHub workflow to build the project whenever new code is pushed to the repository on GitHub.

1. Clone this repository somewhere on your desktop
2. Open a terminal window in the newly cloned repo and enter the following command: `gradle init`
3. When prompted select the following options to initialize the Gradle project:
  * Select type of project to generate:
     * **2: application**
  * Select implementation language:
     * **3: Java**
  * Select build script DSL:
     * **1: Groovy**
  * Select test framework:
     * **4: JUnit Jupiter**
  * Project name:
     * **Use default [project-name]**
  * Source package:
     * **edu.cscc**
4. Once the Gradle project is generated, enter the following Git commands into the terminal to push your work to the remote:
```
git add .
git commit -m "Initial commit. Added Gradle project."
git push
```

## Creating a workflow

5. Return to GitHub and select the "Actions" tab at the top of the repository.
  * You will see a suggestion to use the "Java with Gradle" workflow. This would work for our purposes, however, we will be making a workflow from scratch for this exercise.
6. Select the option "set up a workflow yourself" at the top of the page
7. Define the `name` for the workflow on the first line:
```
name: Build Gradle project
```

### Question 1 (Provide your answers in GitHubActionsQuestions.txt):
>Is the name: syntax required for a GitHub Actions workflow? What does it do? What are the advantages of having a name defined for a workflow file?

8. Define the Action that will act as the trigger for this workflow. In this case it will be every time a new commit(s) is pushed to the remote:
```
on:
  push:
```
9. Define the job that this workflow will perform. Our new job will be named `build-gradle-project`:
```
jobs:
  build-gradle-project:
```
10. Define what container the job will run in. This should be nested under the `build-gradle-project` definition:
```
runs-on: ubuntu-latest
```
* **NOTE:** This will create a new ubuntu container for the code to be ran in. This container will checkout the code and build our project defined in the following steps.
11. Define a `steps:` clause nested under the `build-gradle-project:` below `runs-on: ubuntu-latest`. Keep in mind proper indentation:

### Question 2:
>What are the advantages of running workflows on runners? How would it differ from running the tests directly on a host? How do runners relate to dockerized containers? 

```
steps:
```
12. Define the steps that the job will perform. Each step is defined by a hyphen `-` with a `name:` label and a `uses:` clause that executes a predefined task. Your work flow will perform the following 3 tasks in order:
  * Check out the project sources from this GitHub repo
```
- name: Checkout project
  uses: actions/checkout@v3
```
  * Setup Gradle in the ubuntu container to prepare the execution of Gradle commands:
```
- name: Setup Gradle
  uses: gradle/gradle-build-action@v2
```
  * Run the project build we created earlier using the Gradle Wrapper
```
- name: Run build with Gradle Wrapper
  run: ./gradlew build
```
### Question 3:
>Why would you want to create a new runner and fresh repository for each build?

13. Rename the work flow to `gradle_build.yml` and then commit your changes by selecting the green commit button in the top-right.
14. When returing to the main repo page, refresh your web-browser and notice that there should be a yellow dot or green checkmark at the top of the file list next to the latest commit id. Return to the Actions tab and you should see that actions has ran the workflow you just added. If you see a red `x`, that means the build failed and you will need to verify that the syntax of your workflow is correct. 

### Question 4:
>Navigate to the "build" job for the most recent workflow. Select the gear icon in the top right and then select the "View raw logs". Copy paste the entire log to the bottom of your GitHubActionsQuestions.txt file
