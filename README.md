# ☕ Maven & DevOps Tools – Complete Quick Reference Guide

> Covers Maven build lifecycle, Tomcat deployment, SonarQube code analysis, and Nexus artifact management — all the essentials for a Java CI/CD pipeline.

---

## 📋 Table of Contents

- [Build Tools Overview](#-build-tools-overview)
- [Maven Core Concepts](#-maven--core-concepts)
- [Maven Build Lifecycle](#-maven-build-lifecycle)
- [Java Archive Types](#-java-archive-types)
- [Installation](#-installation-amazon-linux--ec2)
- [Quick Start](#-quick-start--generate--build-a-project)
- [Maven Repository Types](#-maven-repository-types)
- [Project Structure](#-project-structure)
- [Key Maven Commands](#-key-maven-commands)
- [Tomcat](#-tomcat)
- [SonarQube](#-sonarqube)
- [Nexus Repository Manager](#-nexus-repository-manager)
- [Full Pipeline Flow](#-full-devops-pipeline-flow)

---

## 📦 Build Tools Overview

| Tool | Language Ecosystem |
|---|---|
| **Gradle** | Java / Android |
| **NAnt** | .NET |
| **PyBuilder** | Python |
| **Maven** | Java (primary focus) |

---

## 🔵 Maven – Core Concepts

### What is Maven?
- **Open Source** build and project management tool
- **Pre-requisite:** Java must be installed
- Manages dependencies, builds, and project lifecycle
- Uses `pom.xml` (Project Object Model) as its configuration file

---

## 🔄 Maven Build Lifecycle (in order)

```
validate  →  compile  →  test  →  package  →  install  →  deploy
```

| Phase | Description |
|---|---|
| `validate` | Validates project structure and configuration |
| `compile` | Compiles `.java` source files |
| `test` | Runs unit tests |
| `package` | Packages compiled code into a distributable format |
| `install` | Installs artifact to local `.m2` cache |
| `deploy` | Deploys artifact to remote repository (Nexus, etc.) |

---

## 📁 Java Archive Types

| Extension | Full Name | Use Case |
|---|---|---|
| `.java` | Java Source File | Raw source code |
| `.jar` | Java Archive | Standard Java applications / libraries |
| `.war` | Web Archive | Web applications (deployed to Tomcat, etc.) |
| `.ear` | Enterprise Archive | Enterprise Java EE applications |

---

## ⚙️ Installation (Amazon Linux / EC2)

### Step 1 – Update System
```bash
sudo yum update -y
```

### Step 2 – Install Java (Amazon Corretto 17 LTS)
```bash
sudo yum install java-17-amazon-corretto-devel -y
java -version
```

### Step 3 – Install Maven
```bash
sudo yum install maven -y
mvn --version
```

### Step 4 – Install Utilities
```bash
yum install tree git -y
```

---

## 🚀 Quick Start – Generate & Build a Project

### 1. Create Project Directory
```bash
mkdir smoke-test
```

### 2. Generate Sample Maven Project
```bash
mvn archetype:generate \
  -DgroupId=com.axis.test \
  -DartifactId=smoke-test \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DinteractiveMode=false
```

### 3. Build the Project
```bash
cd smoke-test
mvn clean package
```
> Output artifacts are placed in the `target/` directory.

### 4. Run the Application
```bash
java -cp target/smoke-test-1.0-SNAPSHOT.jar com.axis.test.App
```

### 5. Build with Explicit Java Version
```bash
mvn clean package -Dmaven.compiler.source=17 -Dmaven.compiler.target=17
```

---

## 🔑 SSH Key Generation (for Git / GitHub)
```bash
ssh-keygen
# Public key: /root/.ssh/id_rsa.pub
```

---

## 🗄️ Maven Repository Types

Maven uses a **three-tier** repository model:

```
Central Repository  →  Remote (Nexus/Artifactory)  →  Local Cache (.m2)
```

| Type | Location | Description |
|---|---|---|
| **Central** | `https://repo.maven.apache.org/maven2/` | Public Maven Central repo |
| **Local** | `/root/.m2` | Local cache on developer machine |
| **Remote** | EC2 / Nexus / Artifactory | Private/corporate artifact server |

**Resolution order:**
1. Maven first checks the **local** `.m2` cache
2. If not found, looks in the **remote** corporate repo
3. Falls back to **Maven Central** if still not found

---

## 📂 Project Structure (after archetype generation)

```
smoke-test/
├── pom.xml                       ← Project Object Model (config file)
└── src/
    ├── main/
    │   └── java/com/axis/test/
    │       └── App.java
    └── test/
        └── java/com/axis/test/
            └── AppTest.java
```

---

## 📝 Key Maven Commands

| Command | Description |
|---|---|
| `mvn validate` | Validate project |
| `mvn compile` | Compile source code |
| `mvn test` | Run tests |
| `mvn package` | Package into JAR/WAR/EAR |
| `mvn clean` | Remove `target/` directory |
| `mvn clean package` | Clean then package |
| `mvn install` | Install artifact to local `.m2` |
| `mvn deploy` | Upload artifact to remote repo (Nexus) |
| `mvn sonar:sonar` | Run SonarQube code analysis |
| `mvn dependency:tree` | Show dependency tree |

---

## 🐱 Tomcat

### What is Tomcat?
Apache Tomcat is an open-source **Java web server and servlet container** used to deploy `.war` files (web applications).

| Detail | Value |
|---|---|
| **Vendor** | Apache Software Foundation |
| **Default Port** | `8080` |
| **Deploys** | `.war` files |
| **Language** | Java |

### Installation (Amazon Linux)
```bash
# Install Java first
sudo yum install java-17-amazon-corretto-devel -y

# Download Tomcat
wget https://downloads.apache.org/tomcat/tomcat-10/v10.1.20/bin/apache-tomcat-10.1.20.tar.gz

# Extract and move
tar -xvzf apache-tomcat-10.1.20.tar.gz
mv apache-tomcat-10.1.20 /opt/tomcat

# Start Tomcat
/opt/tomcat/bin/startup.sh
```

### Key Directory Structure
```
/opt/tomcat/
├── bin/          ← startup.sh, shutdown.sh
├── conf/
│   └── tomcat-users.xml   ← User roles & credentials
├── webapps/      ← DROP your .war files here
│   └── ROOT/
└── logs/
    └── catalina.out       ← Main log file
```

### Deploy a WAR File
```bash
# Simply copy the WAR into the webapps directory
cp target/myapp.war /opt/tomcat/webapps/

# Tomcat auto-extracts and deploys it
# Access at: http://<server-ip>:8080/myapp
```

### Configure Admin User (`conf/tomcat-users.xml`)
```xml
<tomcat-users>
  <role rolename="manager-gui"/>
  <role rolename="admin-gui"/>
  <user username="admin" password="admin123"
        roles="manager-gui,admin-gui"/>
</tomcat-users>
```

### Key Commands
| Command | Description |
|---|---|
| `/opt/tomcat/bin/startup.sh` | Start Tomcat |
| `/opt/tomcat/bin/shutdown.sh` | Stop Tomcat |
| `tail -f /opt/tomcat/logs/catalina.out` | View live logs |

### 📝 Tomcat Notes
> - WAR files dropped into `webapps/` are **auto-deployed** — no restart needed
> - Default port is `8080` — ensure this is open in your Security Group
> - Manager GUI available at `http://<ip>:8080/manager/html` (requires admin user setup)
> - Always check `catalina.out` first when troubleshooting deployment failures
> - For production, run Tomcat as a dedicated non-root system user

---

## 🔍 SonarQube

### What is SonarQube?
SonarQube is an open-source platform for **continuous code quality inspection**. It detects bugs, vulnerabilities, code smells, and tracks code coverage.

| Detail | Value |
|---|---|
| **Vendor** | SonarSource |
| **Default Port** | `9000` |
| **Default Login** | `admin / admin` |
| **Database** | H2 (dev) / PostgreSQL (production) |
| **Language** | Java |

### Installation (Docker – Recommended)
```bash
docker run -d \
  --name sonarqube \
  -p 9000:9000 \
  sonarqube:lts-community
```

> Access at: `http://<server-ip>:9000`

### Installation (Amazon Linux – Manual)
```bash
# Install Java
sudo yum install java-17-amazon-corretto-devel -y

# Download SonarQube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.4.1.88267.zip
unzip sonarqube-10.4.1.88267.zip
mv sonarqube-10.4.1.88267 /opt/sonarqube

# Start SonarQube (must NOT run as root)
/opt/sonarqube/bin/linux-x86-64/sonar.sh start
```

### Run Analysis via Maven
```bash
mvn sonar:sonar \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=<your-sonar-token>
```

### `pom.xml` SonarQube Properties
```xml
<properties>
  <sonar.host.url>http://localhost:9000</sonar.host.url>
  <sonar.projectKey>my-project</sonar.projectKey>
  <sonar.projectName>My Project</sonar.projectName>
  <sonar.sources>src/main/java</sonar.sources>
  <sonar.tests>src/test/java</sonar.tests>
</properties>
```

### Key Concepts

| Concept | Description |
|---|---|
| **Bug** | Code that is broken or will break |
| **Vulnerability** | Security-related issue |
| **Code Smell** | Maintainability issue (not a bug, but bad practice) |
| **Coverage** | % of code covered by unit tests |
| **Duplications** | Copy-pasted code blocks |
| **Quality Gate** | Pass/fail threshold — blocks deployment if failed |

### 📝 SonarQube Notes
> - Always set a **Quality Gate** — it blocks pipeline progress if code quality drops below threshold
> - Use a **SonarQube token** (not username/password) for CI/CD integration
> - SonarQube **cannot run as root** — create a dedicated `sonar` system user for manual installs
> - For teams, use **SonarQube Community Edition** (free) or upgrade to Developer Edition for branch analysis
> - Integrate with Jenkins using the **SonarQube Scanner** plugin for automatic gate checks
> - Reports are retained per project at `http://<ip>:9000/projects`

---

## 📦 Nexus Repository Manager

### What is Nexus?
Sonatype Nexus is an **artifact repository manager** that stores, organizes, and distributes build artifacts (JARs, WARs, Docker images, npm packages, etc.).

| Detail | Value |
|---|---|
| **Vendor** | Sonatype |
| **Default Port** | `8081` |
| **Default Login** | `admin / admin123` |
| **Stores** | JAR, WAR, Docker, npm, PyPI, NuGet |

### Installation (Docker – Recommended)
```bash
docker run -d \
  --name nexus \
  -p 8081:8081 \
  sonatype/nexus3
```

> Access at: `http://<server-ip>:8081`

### Installation (Amazon Linux – Manual)
```bash
# Download Nexus
wget https://download.sonatype.com/nexus/3/nexus-3.67.1-01-unix.tar.gz
tar -xvzf nexus-3.67.1-01-unix.tar.gz
mv nexus-3.67.1-01 /opt/nexus

# Start Nexus
/opt/nexus/bin/nexus start
```

### Repository Types

| Type | Description | Use Case |
|---|---|---|
| **hosted** | Stores your own artifacts | Upload project WARs/JARs |
| **proxy** | Caches remote repos (Maven Central) | Speed up downloads |
| **group** | Combines multiple repos under one URL | Single URL for all deps |

### Common Repositories to Create

| Repo Name | Type | Version Policy |
|---|---|---|
| `maven-releases` | Maven2 (hosted) | Release |
| `maven-snapshots` | Maven2 (hosted) | Snapshot |
| `maven-central` | Maven2 (proxy) | Mixed |
| `maven-public` | Maven2 (group) | Mixed |

### Configure Maven to Use Nexus

**`~/.m2/settings.xml`:**
```xml
<settings>
  <servers>
    <server>
      <id>nexus</id>
      <username>admin</username>
      <password>admin123</password>
    </server>
  </servers>
  <mirrors>
    <mirror>
      <id>nexus</id>
      <mirrorOf>*</mirrorOf>
      <url>http://localhost:8081/repository/maven-public/</url>
    </mirror>
  </mirrors>
</settings>
```

**`pom.xml` – Distribution Management:**
```xml
<distributionManagement>
  <repository>
    <id>nexus</id>
    <url>http://localhost:8081/repository/maven-releases/</url>
  </repository>
  <snapshotRepository>
    <id>nexus</id>
    <url>http://localhost:8081/repository/maven-snapshots/</url>
  </snapshotRepository>
</distributionManagement>
```

### Deploy Artifact to Nexus
```bash
mvn clean deploy
```

### 📝 Nexus Notes
> - **Releases** are immutable — once deployed, they cannot be overwritten (by default)
> - **Snapshots** are mutable — use `-SNAPSHOT` suffix in your version for dev builds
> - Version naming convention: `1.0.0` → release, `1.0.0-SNAPSHOT` → snapshot
> - Nexus acts as a **proxy cache** for Maven Central — speeds up builds significantly in CI
> - Always store Nexus credentials in `settings.xml`, never in `pom.xml` (security risk)
> - The **Blob Store** is where Nexus physically stores artifacts — back it up regularly
> - Admin password file location (first run): `/nexus-data/admin.password`

---

## 🔄 Full DevOps Pipeline Flow

```
Developer
    │
    │  git push
    ▼
GitHub / GitLab
    │
    │  Webhook trigger
    ▼
Jenkins Pipeline
    │
    ├──► mvn clean package   →   target/*.war  (Build)
    │
    ├──► mvn sonar:sonar     →   SonarQube :9000  (Code Quality)
    │         │
    │         └── Quality Gate PASS? ──── NO ──► ❌ Pipeline Fails
    │                   │
    │                  YES
    │
    ├──► mvn deploy          →   Nexus :8081  (Artifact Store)
    │
    └──► cp *.war /webapps   →   Tomcat :8080  (Deploy & Serve)
                                      │
                                      ▼
                          🌐 http://<server>:8080/myapp
```

### Service Port Summary

| Service | Port | URL |
|---|---|---|
| **Tomcat** (App) | `8080` | `http://<ip>:8080/<app-name>` |
| **SonarQube** | `9000` | `http://<ip>:9000` |
| **Nexus** | `8081` | `http://<ip>:8081` |
| **Jenkins** | `8080` | `http://<ip>:8080` |
