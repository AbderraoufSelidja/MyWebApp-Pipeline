# Continuous Integration of a Java API with Jenkins

## Project Overview
This project demonstrates the implementation of a **Continuous Integration (CI)** process for a Java API using **Gradle** and **Jenkins**.  
It covers the complete pipeline from building and testing to deployment and notifications, ensuring code quality and automation at every stage.

---

## Features
- **Gradle Project Structure** with:
  - `src` folder for source code
  - `Features` folder for Cucumber test features
  - `build.gradle` with **JaCoCo**, **SonarQube**, and **maven-publish** plugins
  - `Jenkinsfile` defining the CI/CD pipeline
- **GitHub Integration** with Webhook for automated builds
- **Multi-Branch Pipeline** setup in Jenkins

---

## CI Pipeline Phases

### 1 Test Phase
- Run unit tests  
- Archive unit test results  
- Generate **Cucumber** test reports  

### 2 Code Analysis
- Analyze code quality using **SonarQube**

### 3 Code Quality Gate
- Verify **SonarQube Quality Gates** status  
- Stop pipeline execution if status is **FAILED**

### 4 Build Phase
- Generate **JAR** file  
- Generate **documentation**  
- Archive JAR and documentation  

### 5 Deploy Phase
- Deploy the generated JAR to [MyMavenRepo](https://mymavenrepo.com/)

### 6 Notification Phase
- Send success notifications via:
  - Email  
  - Slack  
- Send failure notifications in case of any pipeline stage error


