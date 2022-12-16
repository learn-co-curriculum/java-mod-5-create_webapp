# Creating a Java Web Application Project

## Learning Goals

- Create a simple Java web application project
- Configure pom.xml to add the Tomcat9 Maven Plugin
- Launch embedded Tomcat9 server
- Load a web application in the browser

## Introduction

An **archetype** is a Maven project templating toolkit.
Maven provides an archtype named `maven-archetype-webapp` that
generates the basic project structure for a Java web application.

Recall that we need a servlet container to run our Java web application.
In this lesson, we will use Maven to add a plugin for running web applications
using an embedded Tomcat9 server.

## Code Along

## Create a new Maven webapp project

We will use Maven to create a new web application project.

1. Open IntelliJ and select from the menu `File/New/Project`.  
2. On the left side, select  `Maven Archtype`.  
3. Select `org.apache.maven.archetypes:maven-archetype-webapp` from the archtype dropdown menu.
4. Enter `serlvets` as the project name.
5. Select the appropriate folder location of where to save the project.
6. Select the version of JDK 11 that you installed.
7. Click the `Create` button.

![maven webapp archtype](https://curriculum-content.s3.amazonaws.com/6036/create-webapp-project/maven_webapp.png)

IntelliJ should generate a project with the following structure:

![initial project structure](https://curriculum-content.s3.amazonaws.com/6036/create-webapp-project/initial_project_structure.png)

Maven did not generate a java folder, which we need for placing our servlet classes.

Right-click the `main` folder and select `New > Directory`.  Name the new directory `java`.

The updated project structure should appear as shown:

![updated project structure](https://curriculum-content.s3.amazonaws.com/6036/create-webapp-project/updated_project_structure.png)  

The `webapp` folder contains a `WEB-INF` folder and a file named `index.jsp`. 

- The `WEB-INF` folder contains a Java web configuration file `web.xml`.
- The `index.jsp` file is a **Java Server Pages** file, which looks similar to HTML
  but allows dynamic content injection into static contents.

## Configure pom.xml

Edit `pom.xml` to tell Maven to use Java 11 (place this after the
version element):

```xml
<!-- Set compiler to Java11 -->
  <properties>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
  </properties>
```

Edit `pom.xml` to add a plugin that lets us run an embedded
Tomcat servlet container (nest this within the build element):

```xml
<!-- The Tomcat9 Maven Plugin -->
    <plugins>
      <plugin>
        <groupId>org.opoo.maven</groupId>
        <artifactId>tomcat9-maven-plugin</artifactId>
        <version>3.0.1</version>
        <configuration>
          <path>/</path>
        </configuration>
      </plugin>
    </plugins>
```

Your updated `pom.xml` should appear as shown:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>org.example</groupId>
  <artifactId>servlets</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>

  <!-- Set compiler to Java11 -->
  <properties>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
  </properties>

  <name>servlets Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <build>
    <finalName>servlets</finalName>

    <!-- Tomcat9 Maven Plugin -->
    <plugins>
      <plugin>
        <groupId>org.opoo.maven</groupId>
        <artifactId>tomcat9-maven-plugin</artifactId>
        <version>3.0.1</version>
        <configuration>
          <path>/</path>
        </configuration>
      </plugin>
    </plugins>

  </build>
</project>

```

Don't forget to reload Maven after making changes to `pom.xml`:

![reload maven icon](https://curriculum-content.s3.amazonaws.com/6036/create-webapp-project/reload_maven.png)


## Run Tomcat Plugin

1. Click on the Maven toolbar to open the Maven panel.
2. Expand the `Plugins` dropdown menu.
3. Expand the `tomcat9` dropdown menu.
4. Double-click on `tomcat9:run` to run the current web application on the embedded Tomcat9 server.

![tomcat plugin](https://curriculum-content.s3.amazonaws.com/6036/create-webapp-project/run_tomcat.png)

The console output should display something similar to this (possibly with some warnings displayed in red):

```text
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------< org.example:servlets >------------------------
[INFO] Building servlets Maven Webapp 1.0-SNAPSHOT
[INFO] --------------------------------[ war ]---------------------------------
[INFO] 
[INFO] >>> tomcat9-maven-plugin:3.0.1:run (default-cli) > process-classes @ servlets >>>
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ servlets ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO] 
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ servlets ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] <<< tomcat9-maven-plugin:3.0.1:run (default-cli) < process-classes @ servlets <<<
[INFO] 
[INFO] 
[INFO] --- tomcat9-maven-plugin:3.0.1:run (default-cli) @ servlets ---
[INFO] Running war on http://localhost:8080/
[INFO] Using existing Tomcat server configuration 
[INFO] create webapp with contextPath: 
```

The Tomcat web container is running and by default listens for requests on port 8080.
Open a browser and enter the URL `http://localhost:8080`.

![Tomcat displaying localhost:8080](https://curriculum-content.s3.amazonaws.com/6036/create-webapp-project/index_jsp.png)

Tomcat is displaying the default home page for the current web application,
which is the file `index.jsp` that resides in the `webapps` directory.
We can also include the file name in the URL `http://localhost:8080/index.jsp`.

The webapps directory is the default document root for the web application.
This is where Tomcat will look for HTML and JSP pages, Javascript and stylesheets,
and other files that must be visible to the client browser.

Normally, the path to `index.jsp` would be `http://localhost:8080/servlets`,
since the IntelliJ project name is `servlets`.  However, if you look
at the Tomcat9 plugin configuration in `pom.xml`, we added an element that
sets the context path to `/`.  This lets us reach files in the web application
without having to add the project name, i.e `http://localhost:8080`.

```xml
 <configuration>
  <path>/</path>
</configuration>
```

Edit `index.jsp` and change the greeting:

```html
<html>
<body>
<h2>Hi There!</h2>
</body>
</html>
```

Save the file and reload the page in the browser:

![index.jsp updated content](https://curriculum-content.s3.amazonaws.com/6036/create-webapp-project/index_jsp_updated.png)

## Conclusion

We used a Maven archtype to generate a web application project.
We also installed the Tomcat9 Maven plugin, which let us launch an embedded Tomcat
server as part of the current web application project. 

## Resources

- [Maven webapp archtype](https://maven.apache.org/archetypes/maven-archetype-webapp/)   
- [Tomcat9 Maven Plugin](https://search.maven.org/artifact/org.opoo.maven/tomcat9-maven-plugin)  
- [Java Server Pages](https://www.oracle.com/java/technologies/jspt.html)
