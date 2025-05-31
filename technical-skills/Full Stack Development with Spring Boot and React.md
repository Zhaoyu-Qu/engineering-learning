
# Overview
For the purpose of self-study, I am developing a small full-stack project. In this project, I will try to learn and use technologies that I am not familiar with, such as Docker, GitHub Actions, Amazon RDS, and more. This document details every step of the project development and the questions and explanations that arise during this process.

# Backend
## Concepts
### Gradle
Gradle is a build automation tool that **manages project dependencies** and **handles the build process**. The configuration is done in the `build.gradle` file. You can use either the Kotlin or Groovy programming languages to write `build.gradle` files. In this project we will be using Groovy.

Below is an example of a Spring Boot project's `build.gradle` file:
```
plugins {  
    id 'java'
    id 'org.springframework.boot' version '3.1.0'
    id 'io.spring.dependency-management' version '1.1.0'
}

group = 'com.packt'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '17'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    testImplementation 'org.springframework.boot:spring-boot-starter-
    test'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

The `build.gradle` file typically contains the following parts:
- **Plugins**: These are modularised code blocks that allow users to easily add new features to the project without having to write code from scratch. Plugins come from Gradle itself (core plugins), [Gradle Plugin Portal](https://plugins.gradle.org), public repositories, or they can be developed by yourself. In the example above, there are three plugins:
  - The Java Plugin: It enables the project to compile and test Java programs. [Doc Page](https://docs.gradle.org/current/userguide/java_plugin.html)
  - Spring Boot: [Doc Page](https://plugins.gradle.org/plugin/org.springframework.boot)
  - Spring Dependency Management: A Gradle plugin that provides Maven-like dependency management functionality. [Doc Page](https://plugins.gradle.org/plugin/io.spring.dependency-management)
- **Repositories**: The repositories block defines the dependency repositories that are used to resolve dependencies. We are using the Maven Central repository, from which Gradle pulls the dependencies.
- **Dependencies**: The block defines the dependencies that are used in the project. The key words before the dependencies are called *configurations*. Each configuration represents the scope of a dependency. For example, the `developmentOnly` configuration means that the dependency should only be used during development but should not be packaged in the final production artifact (like a .jar or .war file). Pre-defined configurations usually come from Gradle plugins. 
  - Note the `plugins` block has a plugin called `org.springframework.boot`, while the `dependencies` block happens to have a dependency called `org.springframework.boot:spring-boot-starter-web`. However, the plugin and the dependency are separate things. The plugin (`org.springframework.boot`) provides Gradle-specific functionality for building and running Spring Boot applications (e.g., packaging the app as an executable JAR, reloading support, configuration conveniences). In comparison, the dependency (`spring-boot-starter-web`) pulls in Java libraries like Spring MVC, Jackson, and Tomcat to allow you to build web applications. Dependencies are part of your application’s runtime. They bring in the actual Java libraries you need for your application to function.
- **Tasks**: The tasks block defines the tasks that are part of the build process, such as testing. [Doc Page](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html)
  - When a user runs `./gradlew build` in the command line, Gradle will execute the `build` task along with any other tasks it depends on.
  - Tasks either come from build scripts or plugins. Gradle provides several default tasks for a project, which are listed by running `./gradlew tasks`. Once we apply a plugin to our project, additional tasks become available.

## Technical Requirements
- Eclipse IDE for Enterprise Java and Web Developers (eclipse-jee-2025-03-R-macosx-cocoa-aarch64.dmg)
- JDK 21

## Implementation Steps
### Spring Initializr
1. https://start.spring.io - a tool that is used to create Spring Boot projects
2. Select Gradle Groovy + Spring Boot 3.1.0 + Jar + Java 21
  - Dependencies:
    - `Spring Web` (Builds web, including RESTful, application using Spring MVC. Uses Apache Tomcat as the default embedded container)
    - `Spring Boot Dev Tools` (Provides fast application restarts, LiveReload, and configurations for enhanced development experience)
  - Metadata:
    - Group: `cloud.sleepjournal`. It's the base package or namespace for your project, usually written in reverse domain name format. For example, the intended domain name is `sleepjournal.cloud`, so you reverse it to `cloud.sleepjournal`. It helps uniquely identify your project among many others. It's also used as the base package in your Java code unless you override it.
    - Artifact: `sleep-journal-backend`. When you build the app, Gradle or Maven will generate a file like sleep-journal-backend-0.0.1-SNAPSHOT.jar. This is also the project's root directory.
    - Name: `Sleep Journal`. This is a human-readable name for the project. It does not affect class names or whatever.
    - Description: `A web application for tracking and analyzing sleep patterns`.
    - Package name: `cloud.sleepjournal.sleep-journal-backend`. This is the root package for all your classes. Spring Boot uses this to perform component scanning, so your @RestController, @Service, etc. must be in this package or sub-packages to be auto-detected.
3. Generate and extract the ZIP package, then open Eclipse
4. In Eclipse, select `File` -> `Import` -> `Gradle` -> `Existing Gradle Project` -> `Browse...` -> `Finish`
5. Run the application from the terminal using `./gradlew bootrun` or run the application using the UI.

#### Notes
- In Eclipse under `Project Explorer`, there is a directory called `JRE System Library [JavaSE-21]`. `JRE System Library` is a virtual directory only visible in `Project Explorer`. It points to the location of the specific JRE used by this project, even though there might be multiple version of JREs installed. `[JavaSE-21]` is the intended JRE version for this project, inferred by Eclipse from the `build.gradle` file. However, Eclipse or Gradle does not guarantee the intended JRE version is used - it just uses the default JRE. You must make sure the intended version of JRE is available and is configured correctly.
- There is another virtual directory called `Project and External Dependencies`, which points to the cached Gradle dependencies. These dependencies are downloaded by Gradle and can be reused by other projects. If you delete the cached Gradle dependencies, Gradle will simply redownload them the next time you build the project.
- On macOS, the default Java version is managed by a tool called `/usr/libexec/java_home.` This allows you to set and switch between Java versions system-wide using the `JAVA_HOME` environment variable.
- Command `/usr/libexec/java_home -v 21` shows the actual location of the JDK 21 on your system, while `which java` shows the the path to the java executable that is currently first in your shell's PATH.