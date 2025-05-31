
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
- Eclipse IDE for Enterprise Java and Web Developers
- 