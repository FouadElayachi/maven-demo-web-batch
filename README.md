# maven-demo-web-batch

# Description

# Getting started

## Build the main project
```
mvn -B archetype:generate -DgroupId=org.falcon.demo -DartifactId=demo -Dversion=1.0-SNAPSHOT -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4
```
## Build the batch module
```
mvn -B archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DgroupId=org.falcon.demo -DartifactId=demo-batch -Dpackage=org.falcon.demo.batch
```
## Build the webapp module
```
mvn -B archetype:generate -DarchetypeArtifactId=maven-archetype-webapp -DarchetypeVersion=1.4 -DgroupId=org.falcon.demo -DartifactId=demo-webapp -Dpackage=org.falcon.demo.webapp
```
## Build the business module
```
mvn -B archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DgroupId=org.falcon.demo -DartifactId=demo-business -Dpackage=org.falcon.demo.business
```
## Build the consumer module
```
mvn -B archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DgroupId=org.falcon.demo -DartifactId=demo-consumer -Dpackage=org.falcon.demo.consumer
```
## Build the model module
```
mvn -B archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DgroupId=org.falcon.demo -DartifactId=demo-model -Dpackage=org.falcon.demo.model
```
## Add dependencies between module
- In the pom.xml file of the main project, we need to add all the modules and there version: 
```
  <!-- ======================================================================= -->
  <!-- Dependencies management -->
  <!-- ======================================================================= -->
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.falcon.demo</groupId>
        <artifactId>demo-batch</artifactId>
        <version>1.0-SNAPSHOT</version>
      </dependency>
      <dependency>
        <groupId>org.falcon.demo</groupId>
        <artifactId>demo-webapp</artifactId>
        <version>1.0-SNAPSHOT</version>
      </dependency>
      <dependency>
        <groupId>org.falcon.demo</groupId>
        <artifactId>demo-business</artifactId>
        <version>1.0-SNAPSHOT</version>
      </dependency>
      <dependency>
        <groupId>org.falcon.demo</groupId>
        <artifactId>demo-consumer</artifactId>
        <version>1.0-SNAPSHOT</version>
      </dependency>
      <dependency>
        <groupId>org.falcon.demo</groupId>
        <artifactId>demo-model</artifactId>
        <version>1.0-SNAPSHOT</version>
      </dependency>
    </dependencies>
  </dependencyManagement>
```
- Add the dependencies in the batch module:
```
    <!-- ======= Modules ======= -->
    <dependency>
      <groupId>org.falcon.demo</groupId>
      <artifactId>demo-business</artifactId>
    </dependency>
    <dependency>
      <groupId>org.falcon.demo</groupId>
      <artifactId>demo-model</artifactId>
    </dependency>
```
- Add the dependencies in the webapp module:
```
    <!-- ======= Modules ======= -->
    <dependency>
      <groupId>org.falcon.demo</groupId>
      <artifactId>demo-business</artifactId>
    </dependency>
    <dependency>
      <groupId>org.falcon.demo</groupId>
      <artifactId>demo-model</artifactId>
    </dependency>
```
- Add the dependencies in the consumer module:
```
    <!-- ======= Modules ======= -->
    <dependency>
      <groupId>org.falcon.demo</groupId>
      <artifactId>demo-model</artifactId>
    </dependency>
```
- Add the dependencies in the business module:
```
    <!-- ======= Modules ======= -->
    <dependency>
      <groupId>org.falcon.demo</groupId>
      <artifactId>demo-consumer</artifactId>
    </dependency>
    <dependency>
      <groupId>org.falcon.demo</groupId>
      <artifactId>demo-model</artifactId>
    </dependency>
```
# Requirements

##### Each of the webapp and batch modules contain a `info.properties` file with `application.version`.
- Create a directory named `resources` in the `src/main` directory of each module.
- Create the file `info.properties`in the `resources`directory for each module and copy the following code inside it:
```
application.version=${project.version}
```
- The version must be taken automaticly while bulding the project. For that, we filter the resource in each module by adding the following code in build of the pom.xml file:
```
    <!-- ======= Resources ======= -->
    <resources>
      <!-- Get the application version  -->
      <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
        <includes>
          <include>info.properties</include>
        </includes>
      </resource>
    </resources>
```

