# maven-demo-web-batch

# Description
maven-demo-web-batch is a beginner friendly tutorial in order to elaborate the main concepts of maven software by using a Multitier architecture.

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

#### Each of the webapp and batch modules contain a `info.properties` file with `application.version`
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
- To test the filtering, copy paste the following code in the App Class of the batch module and run the class:
```
    Properties vProp = new Properties();
    InputStream vInputStream = null;
    try {
      vInputStream = App.class.getResourceAsStream("/info.properties");
      vProp.load(vInputStream);
    } finally {
      if(vInputStream != null) {
        vInputStream.close();
      }
    }

    System.out.println("Application version: " + vProp.getProperty("application.version", "?"));
```

#### Use profiles
- Generate a JAR of the sources for each module during the package phase if this profile `with-sources` is activated.
- To do that, we need to define a profile with `with-sources` id, and activate the plugin `maven-source-plugin` on `package` phase wich have `jar` as goal. Copy the following code on `pom.xml` of the main project.
````
  <!-- =============================================================== -->
  <!-- Profiles -->
  <!-- =============================================================== -->
  <profiles>
    <!-- Generate a JAR of the sources for each module during the package phase if this profile is activated -->
    <profile>
      <id>with-sources</id>
      <build>
        <plugins>
          <plugin>
            <artifactId>maven-source-plugin</artifactId>
            <executions>
              <execution>
                <id>source-jar</id>
                <phase>package</phase>
                <goals>
                  <goal>jar</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
        </plugins>
      </build>
    </profile>
  </profiles>
````
- To test it, run the command `mvn clean package -P with-sources`.

#### Generate a jar for `Javadoc` for each module during the `package`phase
- Add the `javadoc` plugin inside the `pluginManagement` tag on the `pom.xml` file of the main project. Copy the following code:
````
  <!-- Javadoc -->
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-javadoc-plugin</artifactId>
    <version>3.2.0</version>
  </plugin>
````
- The Javadoc must be generated with the `goal` jar during the `package`phase. To do that, add the following to the `build` tag inside the `pom.xml` of the main project:
````
<!-- ======= Generate JavaDoc ======= -->
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-javadoc-plugin</artifactId>
        <executions>
          <execution>
            <id>javadoc-jar</id>
            <phase>package</phase>
            <goals>
              <goal>jar</goal>
            </goals>
          </execution>
        </executions>
    </plugin>
  </plugins>
````

#### Generate `tar.gz/zip`files for the batch module
- In order to generate the `tar.gz/zip` files that contains jar of the batch module and their dependencies during the package phase, first of all we need to add to add the following plugins inside the `pluginManagement` tag of the main `pom.xml`:
````
  <!-- Assembly -->
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-assembly-plugin</artifactId>
    <version>2.2-beta-5</version>
  </plugin>
````
- Specify the configuration and the goal of the plugin inside the `pom.xml` of the batch module.
````
<!-- Assembly -->
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-assembly-plugin</artifactId>
    <configuration>
      <descriptors>
        <descriptor>src/assembly/archive.xml</descriptor>
      </descriptors>
    </configuration>
    <executions>
      <execution>
        <id>assembly-archive</id>
        <phase>package</phase>
        <goals>
          <goal>single</goal>
        </goals>
      </execution>
    </executions>
  </plugin>
````
- Define the assembly descriptor by creating an `archive.xml` inside `src/assembly/` of the batch module.
````
<assembly xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns="http://maven.apache.org/ASSEMBLY/2.1.0"
          xsi:schemaLocation="http://maven.apache.org/ASSEMBLY/2.1.0 http://maven.apache.org/xsd/assembly-2.1.0.xsd"
>
    <id>archive</id>

    <formats>
        <format>tar.gz</format>
        <format>zip</format>
    </formats>

    <dependencySets>
        <dependencySet>
            <scope>runtime</scope>
        </dependencySet>
    </dependencySets>

</assembly>
````
- Spicify the main class that will be added in the jar generated inside the archive for the batch module.
````
  <!-- Jar creation -->
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <configuration>
      <archive>
        <manifest>
          <mainClass>org.falcon.demo.batch.App</mainClass>
          <!-- list all the dependencies jar -->
          <addClasspath>true</addClasspath>
          <classpathPrefix></classpathPrefix>
        </manifest>
      </archive>
    </configuration>
  </plugin>
````

#### Generate a website for our project
- First of all, create a `src/site/site.xml` file inside the main directory(demo) to describe our website. Copy the following code and add it to the file:
````
<project xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://maven.apache.org/DECORATION/1.7.0"
         xsi:schemaLocation="http://maven.apache.org/DECORATION/1.7.0 http://maven.apache.org/xsd/decoration-1.7.0.xsd"
>
    <!-- ==== Define the skin(appearance) of my website  ==== -->
    <!-- use the fluido template -->
    <skin>
        <groupId>org.apache.maven.skins</groupId>
        <artifactId>maven-fluido-skin</artifactId>
        <version>1.9</version>
    </skin>

    <body>
        <!-- ==== Breadcrumb trail  ==== -->
        <breadcrumbs>
            <item name="Home" href="index.html" />
        </breadcrumbs>


        <!-- ==== Menus  ==== -->
        <!-- Menu to the parent project -->
        <menu ref="parent" inherit="top" />
        <!-- Menu to the different reports -->
        <menu ref="reports" inherit="top" />
        <!-- Menu to the different modules -->
        <menu ref="modules" inherit="top" />

        <!-- ==== Menu to the documentation  ==== -->
        <menu name="Documentation">
            <!-- Item to the setup page  -->
            <item name="Setup" href="setup.html" />
        </menu>
    </body>

    <!-- ==== Change the default positions of the publish date and version from the left to the right  ==== -->
    <publishDate position="right" />
    <version position="right" />
</project>
````
- Add a documentation page for the setup using the `markdown` format by creating a `setup.md` file inside the `src/site/markdown` directory, and copy paste the following code:
````
## Setup

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
````
- Move on to the `pom.xml` of the parent project, and define the deployment `url` for the website. We achieve it thanks to the distributionManagement section:
````
  <!-- =============================================================== -->
  <!-- Distribution management -->
  <!-- =============================================================== -->
  <distributionManagement>
    <site>
      <id>website-demo</id>
      <url>scp://localhost/tmp/</url>
    </site>
  </distributionManagement>
````
- In the pluginManagement section, add the `maven-site-plugin`:
````
  <!-- website generation -->
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-site-plugin</artifactId>
    <version>3.3</version>
    <configuration>
      <!-- website language -->
      <locales>en</locales>
    </configuration>
  </plugin>
````
- Finally, we generate some reports for our website. We need to add a new section in the `pom.xml` of parent project named `reporting`:
  ````
  <!-- =============================================================== -->
  <!-- Reporting -->
  <!-- =============================================================== -->
  <reporting>
    <plugins>
      <!-- ======= Report for general information of the project ======= -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-project-info-reports-plugin</artifactId>
        <version>3.0.0</version>
        <!-- Show only the reports mentioned in the reportSets section -->
        <reportSets>
          <reportSet>
            <reports>
              <report>index</report>
              <report>summary</report>
              <report>plugins</report>
              <report>dependencies</report>
            </reports>
          </reportSet>
        </reportSets>
      </plugin>

      <!-- ======= Report for the tests ======= -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-report-plugin</artifactId>
        <version>3.0.0-M5</version>
        <configuration>
          <linkXRef>false</linkXRef>
        </configuration>
        <reportSets>
          <!-- aggregate the test reports of all the modules -->
          <reportSet>
            <id>aggregate</id>
            <reports>
              <report>report</report>
            </reports>
            <!-- Don't execute the reportSet on sub modules -->
            <inherited>false</inherited>
            <configuration>
              <aggregate>true</aggregate>
            </configuration>
          </reportSet>
          <reportSet>
            <id>modules</id>
            <inherited>true</inherited>
            <reports>
              <report>report</report>
            </reports>
            <configuration>
              <aggregate>false</aggregate>
            </configuration>
          </reportSet>
        </reportSets>
      </plugin>
    </plugins>
  </reporting>
  ````



