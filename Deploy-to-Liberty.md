# Boost your Spring Boot app with Open Liberty

These instructions are an optional step in the main QuickLab flow. Be sure to complete the [Introduction](README.md#introduction) section before following these steps.

## Deploy the Spring Boot app to Liberty

Since Liberty 18.0.0.2, Spring Boot applications can be directly deployed to Liberty as uber jars. Deploying the application to a server is useful when you already have an application server created or want to have a consistent management approach for applications which use different programming models like Spring, Java EE and MicroProfile.

These instructions use the Boost maven plugin to install Liberty and deploy the application to a managed runtime.

### Install Liberty

Inspect the `pom.xml` - the Boost maven plugin has already been configured.

```xml
  <plugin>
    <groupId>io.openliberty.boost</groupId>
        <artifactId>boost-maven-plugin</artifactId>
        <version>0.1</version>
  </plugin>
```

Use the Boost plugin to start Liberty.

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
