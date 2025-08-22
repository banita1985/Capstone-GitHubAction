# 🚀 Migrating Jenkins-based CI/CD Pipeline to GitHub Actions

This project demonstrates the migration of a **CI/CD pipeline from
Jenkins to GitHub Actions**. The objective is to modernize the CI/CD
process, reduce infrastructure dependencies, and take advantage of
GitHub's cloud-native automation platform.

------------------------------------------------------------------------

## 📌 Pipeline Description

### **Jenkins Pipeline (Before Migration)**

The original pipeline in Jenkins was structured as follows:\
1. **Checkout Source Code** -- Pull code from GitHub.\
2. **Set up Java/Maven** -- Configure build environment.\
3. **Build & Test** -- Compile the project and run tests with Maven.\
4. **SonarQube Scan** -- Perform code quality analysis.\
5. **Archive Artifacts** -- Store generated JAR files for deployment.\
6. **Docker Build & Push** -- Create Docker image and push to
DockerHub.\
7. **Deploy** -- Deploy image to the environment.

------------------------------------------------------------------------

### **GitHub Actions Pipeline (After Migration)**

The pipeline has been fully migrated into **two jobs** (`CI` and `CD`)
defined in `.github/workflows/ci-cd.yml`.

#### ✅ **Continuous Integration (CI)**

-   Triggered on push to `master`.\
-   Steps:
    -   Checkout repo\
    -   Set up JDK 17\
    -   Cache Maven dependencies\
    -   Build with Maven\
    -   Run SonarQube scan (via SonarCloud)\
    -   Upload JAR artifact for deployment

#### ✅ **Continuous Deployment (CD)**

-   Runs after CI (`needs: CI`).\
-   Steps:
    -   Checkout repo\
    -   Download JAR artifact from CI job\
    -   Login to Docker Hub\
    -   Build Docker image with JAR + Dockerfile\
    -   Push Docker image to Docker Hub
------------------------------------------------------------------------

## 🔄 Migration Summary

-   **Jenkinsfile** stages were replicated as **jobs** in GitHub
    Actions.\
-   **SonarQube plugin** in Jenkins → replaced with `mvn sonar:sonar` in
    GitHub Actions.\
-   **Artifact archiving** in Jenkins → replaced with
    `actions/upload-artifact` + `actions/download-artifact`.\
-   **Docker build & push** → migrated with `docker/login-action` +
    native `docker build/push`.\
-   Secrets moved from **Jenkins credentials** → **GitHub Secrets**.

------------------------------------------------------------------------

## ▶️ How to Run the Pipeline

1.  Push your code to the `master` branch.\
2.  Ensure the following secrets are configured in your GitHub repo:
    -   `SONAR_TOKEN` → for SonarCloud analysis.\
    -   `DOCKER_USERNAME` → your DockerHub username.\
    -   `DOCKER_PASSWORD` → your DockerHub password/token.\
3.  Navigate to the **Actions tab** in your GitHub repo to monitor
    pipeline runs.\
4.  On success:
    -   ✅ JAR artifact is created & uploaded.\
    -   ✅ Docker image is built & pushed to DockerHub as
        `<DOCKER_USERNAME>/capstone:1.0`.


✅ With this migration, the CI/CD pipeline is now **fully automated
inside GitHub Actions**, simplifying the workflow and eliminating the
need for a dedicated Jenkins server.

