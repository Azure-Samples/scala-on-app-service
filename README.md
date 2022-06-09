# Scala Play Framework Example for App Service

This example is based off of the [Play Framework Hello World Tutorial](https://github.com/playframework/play-samples/tree/2.8.x/play-scala-hello-world-tutorial). 

To follow the steps in this tutorial you will need the following tools installed locally: 

* [Java 11](https://docs.microsoft.com/en-us/java/openjdk/download#openjdk-11)
* [sbt v1.3.4 or greater](http://www.scala-sbt.org/download.html)
* [Maven](https://maven.apache.org/install.html) (Or install the Azure CLI)
* [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) (Or use Maven)

To check your sbt version, enter the following in a command window:

```bash
sbt sbtVersion
```

## Build and run the project

This example Play project was created from a seed template. It includes all Play components and an Akka HTTP server. The project is also configured with filters for Cross-Site Request Forgery (CSRF) protection and security headers.

To build and run the project:

1. Use a command window to change into the example project directory, for example: `cd scala-on-app-service`

2. Build the project. Enter: `sbt run`. The project builds and starts the embedded HTTP server. Since this downloads libraries and dependencies, the amount of time required depends partly on your connection's speed.

3. After the message `Server started, ...` displays, enter the following URL in a browser: <http://localhost:80>

The Play application responds: `Welcome to the Hello World Scala POC Tutorial!`


## Deploy as JAR file

To build and deploy a .jar file executable for a Java 11 runtime using sbt assembly: 

1. run `sbt assembly` from the project root

2. run `java -jar target/scala-2.13/<project-name>-assembly-<version>.jar` to test the app locally using an executable .jar file. 

3. run `sbt publish` to generate a pom file for the project. The last line of `build.sbt` handles maven repo creation for publishing.

4. copy the newly created pom file to the project root, for example: 

```bash
    cp maven-repo/com/example/scala-play-example_2.13/1.0/scala-play-example_2.13-1.0.pom pom.xml
```

5. configure the webapp using the appropriate azure plugin for maven

```bash
    mvn com.microsoft.azure:azure-webapp-maven-plugin:2.5.0:config
```

6. update the app name and .jar file location in the newly modified pom.xml, for example: 

```xml
    <appName>Scala-App-Name</appName>
```

```xml
    <deployment>
        <resources>
            <resource>
                <directory>${project.basedir}/target/scala-2.13</directory>
                <includes>
                  <include><app-name>-assembly-1.0.jar</include>
                </includes>
            </resource>
        </resources>
    </deployment>
```

7. deploy to App Service with `mvn azure-webapp:deploy`

8. update the application by running `sbt assembly` followed by `mvn azure-webapp:deploy`
