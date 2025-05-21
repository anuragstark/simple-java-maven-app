# Simple Java Maven App with Jenkins CI/CD Pipeline

This repository contains a simple Java application built with Maven and integrated with Jenkins for continuous integration and deployment.

## Project Overview

This is a basic Java Maven application that includes unit tests. The project is integrated with Jenkins to automate the build, test, and deployment processes.

## Jenkins Pipeline Workflow

The Jenkins pipeline for this project is configured to perform the following steps:

### 1. Source Code Management
- **Repository URL**: https://github.com/anuragstark/simple-java-maven-app.git
- **Type**: Git

### 2. Build JAR using Maven
- **Step Type**: Invoke top-level Maven targets
- **Maven Version**: Jenkins Maven installation
- **Goals**: `-B -DskipTests clean package`
- **Description**: This step compiles the code and packages it into a JAR file without running tests

### 3. Run Tests
- **Step Type**: Invoke top-level Maven targets
- **Maven Version**: Jenkins Maven installation
- **Goals**: `test`
- **Description**: This step runs all JUnit tests in the project

### 4. Deploy JAR Locally
- **Step Type**: Execute shell
- **Command**:
  ```bash
  echo "Deployment STEP"
  java -jar /var/lib/jenkins/workspace/maven-demo/target/my-app-1.0-SNAPSHOT.jar
  ```
- **Description**: This step executes the built JAR file to verify it works correctly

### 5. Publish Test Results
- **Step Type**: Post-build Action
- **Action**: Publish JUnit test result report
- **Test report XMLs**: `target/surefire-reports/*.xml`
- **Description**: This step publishes the test results and provides a graphical representation

### 6. Email Notification
- **Step Type**: Post-build Action
- **Action**: Email notification
- **Recipients**: Configured email address
- **Description**: Sends email notifications about build results


## How to Run the Project Locally

### Prerequisites
- Java JDK 17 or higher
- Maven 3.6.0 or higher
- Git

### Steps
1. Clone the repository:
   ```bash
   git clone https://github.com/anuragstark/simple-java-maven-app.git
   cd simple-java-maven-app
   ```

2. Build the project:
   ```bash
   mvn clean package
   ```

3. Run the tests:
   ```bash
   mvn test
   ```

4. Run the application:
   ```bash
   java -jar target/my-app-1.0-SNAPSHOT.jar
   ```

## Setting Up the Jenkins Pipeline

### Jenkins Requirements
- Jenkins server with Maven integration
- Git plugin
- JUnit plugin
- Email Extension plugin

### Configuration Steps

1. **Create a new Jenkins job**:
   - Select "Freestyle project"
   - Enter a name for your job (e.g., "maven-demo")

2. **Configure Source Code Management**:
   - Select Git
   - Enter repository URL: https://github.com/anuragstark/simple-java-maven-app.git
   - Configure branch if needed

3. **Configure Build Steps**:
   - Add build step "Invoke top-level Maven targets"
   - Select your Maven installation
   - Enter goals: `-B -DskipTests clean package`
   - Add another build step "Invoke top-level Maven targets"
   - Enter goals: `test`
   - Add build step "Execute shell"
   - Enter command:
     ```bash
     echo "Deployment STEP"
     java -jar /var/lib/jenkins/workspace/maven-demo/target/my-app-1.0-SNAPSHOT.jar
     ```

4. **Configure Post-build Actions**:
   - Add post-build action "Publish JUnit test result report"
   - Enter test report XMLs pattern: `target/surefire-reports/*.xml`
   - Add post-build action "Email Notification"
   - Configure recipients and SMTP settings in Jenkins global configuration

5. **Save and Build**:
   - Save the job configuration
   - Trigger a build to test the setup


