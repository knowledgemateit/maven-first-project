# Maven Build Tool – Quick Reference Guide

## 📦 Overview: Build Tools

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

---

## 🔄 Maven Build Lifecycle (in order)

```
validate  →  compile  →  test  →  package
```

| Phase | Description |
|---|---|
| `validate` | Validates project structure and configuration |
| `compile` | Compiles `.java` source files |
| `test` | Runs unit tests |
| `package` | Packages compiled code into a distributable format |

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
```

---

## 🗄️ Maven Repository Types

Maven uses a **three-tier** repository model:

```
Central Repository  →  Remote (EC2/Nexus)  →  Local Cache
```

| Type | Location | Description |
|---|---|---|
| **Central** | `https://repo.maven.apache.org/maven2/` | Public Maven Central repo |
| **Local** | `/root/.m2` | Local cache on developer machine |
| **Remote** | EC2 / Nexus / Artifactory | Private/corporate artifact server |

### How it works:
1. Maven first checks the **local** `.m2` cache
2. If not found, looks in the **remote** corporate repo
3. Falls back to **Maven Central** if still not found

---

## 📂 Project Structure (after archetype generation)

```
smoke-test/
├── pom.xml                  ← Project Object Model (config file)
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
