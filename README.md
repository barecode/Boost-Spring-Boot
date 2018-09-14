# Boost your Spring Boot app with Open Liberty

Spring Boot is a popular programming model for building cloud native Java applications. Liberty has supported running Spring framework applications for a long time, and supports running Spring Boot applications packaged as a WAR file. Spring Boot makes it easy to build WAR files, and that's great for situations where you need to deploy to an app server. However, there are limitations (such as not respecting some of the annotated configuration) when deploying as a WAR file. This lab will deploy Spring Boot applications to Liberty and create an uber JAR and Docker image using Open Liberty.

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

Even though the application is launched directly as a JAR, there is still an embedded application server inside. The application server, in this case Tomcat, provides the core technologies needed to host a web-based Java app. This is known as a servlet container, after the Java EE servlet specification.

The Spring Boot application can be directly deployed to Liberty as an uber jar since Liberty 18.0.0.2. Deploying the application to a server is useful when you already have a bunch of application servers already created, or have to manage applications which use different programming models like Spring, Java EE and MicroProfile.

If you would like to deploy to Liberty follow [these instructions](Deploy-to-Liberty.md) use the Boost maven plugin to install Liberty and deploy the application to a managed runtime.  However, there are many steps which make the developer experience more complicated than it should be. Enter the Liberty uber jar.

## The Liberty uber jar

These steps replace the embedded Tomcat server in the Spring Boot uber jar with an embdded Open Liberty server.

### The build

Add the following to the end of `pom.xml` build plugins section. The plugin needs to be included after the `spring-boot-maven-plugin` as it uses the output from the Spring Boot build.

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

INSERT SOMETHING HERE

## Create a Liberty based Docker image

ABC

### The build

Branch: `git checkout demo3-dockerize`
1. Add the following to the end of pom.xml build plugins section:
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
2. Run `./mvnw clean install`

### Run the app

The application 3. Run the Docker image

Run `docker run -p 9080:9080 spring-petclinic`

Browse to `http://localhost:9080/`

INSERT HERE

### Summary

You've seen how easy it is to use Liberty in your Spring Boot project, and to build Docker images based on Open Liberty and Open J9. Liberty ♥ Spring
