---
title: "Build Tool: SBT, Maven, Gradle"
date: 2024-12-28
description: "Build Tool: SBT, Maven, Gradle"
draft: True
categories: ["Engineering", "Build Tool"]
tags: ["SBT", "Maven", "Gradle", "Build Tool", "Scala", "Java"]
author: "Lisa Lin"

showToc: true
TocOpen: false
draft: true
hidemeta: false
comments: false
description: ""
canonicalURL: "https://lisa06010416.github.io/ShareNotes/posts/programming-language/scala/scala-for-data-scientists/"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/Lisa06010416/HugoEngineer/tree/master/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---



| Feature           | SBT                   | Maven                  | Gradle                          |
|------------------|----------------------|------------------------|--------------------------------|
| **Designed For**  | Scala projects       | Java & Scala projects  | Java, Scala, Kotlin projects  |
| **Configuration** | `.sbt` (Scala-based) | `pom.xml` (XML-based)  | `build.gradle` (Groovy/Kotlin) |
| **Dependency Mgmt** | Built-in (Maven Central, Ivy) | Maven Central | Maven, Ivy, Custom Repos |
| **Compilation**   | Incremental & fast   | Conventional, slower   | Parallel & incremental        |
| **Flexibility**   | High                 | Low                    | Very High                     |
| **Performance**   | Fast                 | Moderate               | Fastest (parallel builds)     |
| **Build Lifecycle** | Flexible, custom tasks | Fixed lifecycle (phases) | Task-based execution |



# SBT (Simple Build Tool)

SBT (Simple Build Tool) is the primary build tool for Scal.

* [How to install sbt](https://www.scala-sbt.org/download/) 
* Create a project by sbt`sbt new scala/scala3.g8`
* [Create by IntelliJ](https://docs.scala-lang.org/getting-started/intellij-track/getting-started-with-scala-in-intellij.html)



A sbt project directory structure which is similar to Java:

```tex
my-scala-project/
 ├── build.sbt
 ├── project/
 ├── src/
 │   ├── main/
 │   │   ├── scala/
 │   │   │   ├── Main.scala
 │   ├── test/
 │   │   ├── scala/
 │   │   │   ├── MainTest.scala
```



## Configuration - build.sbt

The `build.sbt` file is the main configuration file for SBT.



### Basic `build.sbt` Configuration

```
ThisBuild / scalaVersion := "3.3.1"

name := "MyScalaProject"
version := "0.1.0"
```





### **Adding Dependencies**

```
libraryDependencies += “{GROUP_ID}” % “{ARTIFACT_ID}” % “{VERSION}” % scope

libraryDependencies += "org.apache.spark" %% "spark-core" % "3.4.1" % provided
```



**Dependency Scopes in SBT**

In SBT, dependencies can be assigned different **scopes**, which determine their availability during different stages of the project lifecycle:

| Scope               | Compilation | Testing | Runtime | Description                                                  |
| ------------------- | ----------- | ------- | ------- | ------------------------------------------------------------ |
| `compile` (default) | ✅           | ✅       | ✅       | Available during compilation, testing, and runtime.          |
| `provided`          | ✅           | ✅       | ❌       | Available during compilation and testing, but not included at runtime. |
| `test`              | ❌           | ✅       | ❌       | Available only during testing, not included in compilation or runtime. |
| `runtime`           | ❌           | ❌       | ✅       | Available only at runtime, not used during compilation.      |







## SBT Commands 

### Basic Commands

| Command        | Description                                                  | Use Case                                                     |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `sbt compile`  | Parses the `build.sbt` configuration.<br/>Downloads and resolves dependencies if there are changes.<br/>Compiles the code in the `src/main/scala` and `src/main/java` directories.<br/>Does not execute the `main` method or run tests. | When modifying Scala code, `build.sbt` settings...           |
| `sbt run`      | First executes `sbt compile` to ensure all code is successfully compiled.<br/>Finds and runs the `main` method.<br/>If multiple `main` methods exist, it prompts you to choose one. | To execute a Scala application                               |
| `sbt clean`    | Cleans the `target/` directory (removes compiled artifacts). | When a full recompilation is needed                          |
| `sbt test`     | Runs all tests.                                              | To ensure code changes do not break tests                    |
| `sbt scalafmt` | Formats Scala code using `scalafmt`.                         | To ensure consistent code formatting                         |
| `sbt`          | Exits SBT and restarts.                                      | When modifying SBT plugins, reloads plugins from `plugins.sbt`. |

### Dependency

| Command              | Description                                | Use Case                          |
| -------------------- | ------------------------------------------ | --------------------------------- |
| `sbt dependencyTree` | Displays the dependency tree               | To check for dependency conflicts |
| `sbt evicted`        | Checks for conflicting dependency versions | To resolve dependency conflicts   |

### Package

| Command            | Description                                                 | Use Case                                                 |
| ------------------ | ----------------------------------------------------------- | -------------------------------------------------------- |
| `sbt package`      | Generates a JAR file                                        | When packaging the project                               |
| `sbt publishLocal` | Publishes the JAR to the local repository                   | For local debugging or dependency use in another project |
| `sbt publish`      | Publishes the JAR to a remote repository (e.g., Maven, Ivy) | When releasing the project for external use              |

## Maven

## Gradle



