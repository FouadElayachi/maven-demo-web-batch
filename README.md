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
