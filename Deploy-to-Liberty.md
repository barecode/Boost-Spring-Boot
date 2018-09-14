# Boost your Spring Boot app with Open Liberty

These instructions are an optional step in the main QuickLab flow. Be sure to complete the [Introduction](README.md#introduction) section before following these steps.

## Deploy the Spring Boot app to Liberty

The Spring Boot application can be directly deployed to Liberty as an uber jar since Liberty 18.0.0.2. Deploying the application to a server is useful when you already have a bunch of application servers already created, or have to manage applications which use different programming models like Spring, Java EE and MicroProfile.

These instructions use the Boost maven plugin to install Liberty and deploy the application to a managed runtime.

### Install Liberty

First, configure the Boost maven plugin in the `pom.xml`. Add the following lines to the build plugins section:

```xml
  <plugin>
    <groupId>io.openliberty.boost</groupId>
        <artifactId>boost-maven-plugin</artifactId>
        <version>0.1</version>
  </plugin>
```

Next, use the Boost plugin to start Liberty as a managed runtime.

Run `./mvnw boost:start`

You can browse to `http://localhost:9080/` to see the Open Liberty welcome page.

### Deploy the Spring Boot uber jar via dropins/spring

Now, we need to configure Liberty with Spring Boot support. This is done by editing the `server.xml` and adding the `springBoot-2.0` and `servlet-4.0` features.

Edit `target/liberty/wlp/usr/servers/BoostServer/server.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<server description="new server">

    <!-- Enable features -->
    <featureManager>
        <feature>springBoot-2.0</feature>
        <feature>servlet-4.0</feature>
    </featureManager>

    <!-- To access this server from a remote client add a host attribute to the following element, e.g. host="*" -->
    <httpEndpoint id="defaultHttpEndpoint"
                  httpPort="9080"
                  httpsPort="9443" />

    <!-- Automatically expand WAR files and EAR files -->
    <applicationManager autoExpand="true"/>
</server>
```

Create the `dropins/spring` directory and copy the application in to deploy it.

Run `mkdir target/liberty/wlp/usr/servers/BoostServer/dropins/spring`

Run `cp target/spring-petclinic-2.0.0.BUILD-SNAPSHOT.jar target/liberty/wlp/usr/servers/BoostServer/dropins/spring`

Now browse to `http://localhost:9080/` to see Spring Petclinic.

When you done exploring Petclinic, stop the server.

Run `./mvnw boost:stop`

The instructions continue at the [the Liberty uber jar section](README.md#the-liberty-uber-jar)
