# Boost your Spring Boot app with Open Liberty

Spring Boot is a popular programming model for building cloud native Java applications. Liberty has supported running Spring framework applications for a long time and supports running Spring Boot applications packaged as a WAR file. Spring Boot makes it easy to build WAR files, and that's great for situations where you need to deploy to an app server. However, there are limitations when deploying as a WAR file - such as only having endpoints served from a single host port config or not being able to use WebFlux APIs. Liberty now supports running Spring Boot applications without repackaing as a WAR. In this this lab you will run Spring Boot applications on Liberty using a variety of methods without the limitations.

## Introduction

This QuickLab shows three different means to use Open Liberty with Spring Boot apps: deploying to Liberty, packaging as a Liberty uber jar, and building a Docker image. The lab uses [Spring Petclinic](https://github.com/spring-projects/spring-petclinic) to demonstrate a Spring Boot application running on Liberty.

### Getting started

To get started, clone the Boost Spring Boot sample repository.

Run `git clone https://github.com/barecode/Boost-Spring-Boot.git`

There are three main parts to the sample:

1. Maven build - `pom.xml`
2. Maven wrapper - `mvnw`
3. Spring Petclinic application - `src/...`

### The build

First, build the Spring Petclinic application.

Run `./mvnw package`

This builds a Spring Boot application with an embedded Tomcat server. Next, run the application and explore Spring Petclinic.

### Run the app

Run `java -jar target/spring-petclinic-2.0.0.BUILD-SNAPSHOT.jar`

Browse to `http://localhost:8080/`

Even though the application is launched directly as a JAR, there is still an embedded application server inside. The application server, in this case Tomcat, provides the core technologies needed to host a Java web app. The app server provides a servlet container as defined by the Java EE servlet specification.

Since Liberty 18.0.0.2, Spring Boot applications can be directly deployed to Liberty as uber jars. Deploying the application to a server is useful when you already have an application server created or want to have a consistent management approach for applications which use different programming models like Spring, Java EE and MicroProfile.

If you would like to deploy the uber jar to Liberty follow [these instructions](Deploy-to-Liberty.md). You will use the Boost maven plugin to install Liberty and deploy the application to a managed runtime.  This process can involve many steps which makes the developer experience more complicated than it otherwise could be. Enter the Liberty uber jar.

## The Liberty uber jar

The Liberty uber jar works like a Spring Boot uber jar, embedding the application server inside the JAR file. These steps use the Boost maven plugin to replace the embedded Tomcat server in the Spring Boot uber jar with an embdded Open Liberty server.

### The build

First, configure the Boost maven plugin in the `pom.xml` with the `package` execution goal. The Boost maven plugin is included after the `spring-boot-maven-plugin` as it uses the output from the Spring Boot build.

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

Run `./mvnw clean package`

### Run the app

Now run the application. You will notice that Liberty starts and then loads the Spring Boot application.

Run `java -jar target/spring-petclinic-2.0.0.BUILD-SNAPSHOT.jar`

Browse to `http://localhost:8080/`

You now have a Spring Boot application running on Open Liberty packaged as an uber jar.

## Create a Liberty based Docker image

Docker is a very popular deployment technology, enabling developers to build applications which behave the same once deployed, delivering on the age old saying 'it works on my machine!'. The Maven boost plugin makes it easy to build a Docker image for your Spring Boot application with minimal knowledge of Docker. Follow the Docker [Getting Started instructions](https://www.docker.com/get-started) to install Docker on your system.

### The build

Configure the Boost maven plugin in the `pom.xml` at the end of build plugins section with the `docker-build` execution goal. The plugin needs to be included after the `spring-boot-maven-plugin` as it uses the output from the Spring Boot build.

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

Run `./mvnw clean install`

This produces a [dual layer](https://openliberty.io/blog/2018/09/12/build-and-push-spring-boot-docker-images.html) Docker image for your Spring Boot application.

### Run the app

Now run the application. You will notice that Liberty starts and then loads the Spring Boot application.

Run `docker run -p 9080:9080 spring-petclinic`

Browse to `http://localhost:9080/`

The Docker image is [optimized](https://openliberty.io/blog/2018/06/29/optimizing-spring-boot-apps-for-docker.html) to seprate the Spring Boot application from the Spring Boot and third-party libraries. This reduces iterative build and deployment time and storage cost. The base image is [Open Liberty](https://openliberty.io/) with [Eclipse Open J9](https://www.eclipse.org/openj9/), two Java runtimes designed and optimized for the cloud.

### Summary

You've seen how easy it is to use Liberty in your Spring Boot project and to build Docker images based on Open Liberty and Open J9.

`Liberty ♥ Spring`
