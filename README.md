# Boost your Spring Boot app with Open Liberty

Spring Boot is a popular programming model for building cloud native Java applications. Open Liberty is a highly efficient and high performing open source runtime designed to run cloud native Java applications. Together, Spring Boot and Open Liberty combine cloud native programming model with a cloud native runtime. Docker has emerged as the cloud native deployment technology of choice because Docker provides a highly efficient packaging and deployment solution for applications. Its easy to build Docker images, but to build efficient Docker images, which minimize deployment time and maximize reuse, care needs to be given to how the Docker image is constructed. Enter the `boost-maven-plugin`. The `boost-maven-plugin` helps you package a Spring Boot application with Open Liberty as an uber jar and to build efficient Docker images.

## Introduction

This QuickLab shows you how to package Spring Boot applications with Open Liberty as an uber jar and an efficient Docker image. The lab uses [Spring Petclinic](https://github.com/spring-projects/spring-petclinic) to demonstrate a Spring Boot application running on Open Liberty.

Open Liberty is a tiny but powerful. Designed from the ground up, Open Liberty is a small, fast, lightweight runtime for cloud native Java applications. Open Liberty's auto-tuning threadpool and advanced security integrations for social login and enterprise security solutions means Open Liberty is enterprise-grade open source.

### Getting started

To get started, clone the Boost Spring Boot sample repository.

Run `git clone https://github.com/barecode/Boost-Spring-Boot.git`

There are three main parts to the sample:

1. Maven build - `pom.xml`
2. Maven wrapper - `mvnw`
3. Spring Petclinic application - `src/...`

The `boost-maven-plugin` is already configured in the `pom.xml` with the `package` execution goal. The `boost-maven-plugin` is included after the `spring-boot-maven-plugin` as it uses the output from the Spring Boot build.

```xml
  <plugin>
    <groupId>io.openliberty.boost</groupId>
    <artifactId>boost-maven-plugin</artifactId>
    <version>0.1</version>
    <executions>
      <execution>
        <goals>
          <goal>package</goal>
        </goals>
     </execution>
   </executions>
  </plugin>
```

The `boost-maven-plugin` packages the Spring Boot application with embedded Open Liberty as an uber jar. Spring Boot applications require an embedded servlet container. The embedded servlet container provides the core technologies needed to host a Java web app. Open Liberty provies the servlet container, along with other capabilities such as SSL/TLS support and websocket support.

## Create an uber jar

### The build

First, build the Spring Petclinic application.

Run `./mvnw package`

This builds the Spring Boot application with an embedded Open Liberty server. Next, run the application and explore Spring Petclinic.

### Run the app

Now run the application.

Run `java -jar target/spring-petclinic-2.0.0.BUILD-SNAPSHOT.jar`

You will notice that Liberty starts and then loads the Spring Boot application.

Browse to `http://localhost:8080/`

You now have a Spring Boot application running on Open Liberty packaged as an uber jar.

## Create a Docker image

Docker is a very popular deployment technology. Docker enables developers to package applications into a deployable image. The Docker image behaves consistently no matter where it is deployed. The consistent behaviour makes Docker ideal for continuous delivery pipelines, because the application behaves the same between development, QA and production. Since the Docker image works the same way everywhere, it makes Docker ideal for delivering on the age old saying 'it works on my machine!' :)

The `boost-maven-plugin` makes it easy to build a Docker image for your Spring Boot application with minimal knowledge of Docker. All that is required is to have Docker installed. Follow the Docker [Getting Started instructions](https://www.docker.com/get-started) to install Docker on your system.

### The build

Configure the `boost-maven-plugin` in the `pom.xml` at the end of build plugins section with the `docker-build` execution goal. The plugin needs to be included after the `spring-boot-maven-plugin` as it uses the output from the Spring Boot build.

```xml
  <plugin>
    <groupId>io.openliberty.boost</groupId>
    <artifactId>boost-maven-plugin</artifactId>
    <version>0.1</version>
    <executions>
      <execution>
        <goals>
          <goal>docker-build</goal>
        </goals>
     </execution>
   </executions>
  </plugin>
```

Run `./mvnw clean package`

This produces a Docker image for your Spring Boot application.

### Run the app

Now run the application.

Run `docker run -p 9080:9080 spring-petclinic`

You will notice that Liberty starts and then loads the Spring Boot application.

Browse to `http://localhost:9080/`

The Docker image is [optimized](https://openliberty.io/blog/2018/06/29/optimizing-spring-boot-apps-for-docker.html) to separate the Spring Boot application from the Spring Boot and third-party libraries. These [two layers](https://openliberty.io/blog/2018/09/12/build-and-push-spring-boot-docker-images.html) reduce iterative build and deployment time, and storage cost. The base image is [Open Liberty](https://openliberty.io/) with [Eclipse OpenJ9](https://www.eclipse.org/openj9/), Java runtimes designed and optimized for the cloud.

## Docker layers matter

### Summary

You've seen how easy it is to use Liberty in your Spring Boot project and to build Docker images based on Open Liberty and Open J9.

`Liberty ♥ Spring`
