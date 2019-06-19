# Spring Boot Notes


### Set Up Environment (Old Notes)

* Install last JDK (jdk-8u172-windows-x64.exe)
* Install Eclipse IDE for Java EE Developers (Oxygen Packages)
* Create Project in Eclipse (New/Project/Gradle Project)
* Update Builship Gradle by using Eclipse Marketplace
* Upgrade Gradle usage in Eclipse by using Gradle Wraper
  * Following directions from: https://docs.gradle.org/current/userguide/gradle_wrapper.html
  * Open GitBash in the Project Directory
  * Execute in that directory 
  ```bash
  ./gradlew wrapper --gradle-version 4.8
  ./gradlew -v
  ```
  * New Gradle Distribution is downloaded at C:\Users\jcabelloc\.gradle\wrapper\dists
* 


# Notes from Chad Darby Course

## Running Spring Boot Apps from Command Line

### Package Spring Boot Project
Located in the Spring Boot Project
```cmd
mvnw package
```
### Run the application - First alternative
```cmd
cd target
java -jar myapp.jar
```
### Run the application - Second alternative
```cmd
cd ..
mvnw spring-boot:run
```

## Inject Custom Application Properties
### Modify the application.properties file
* for example:
```
coach.name=Juan Perez
team.name=The Best Team
```
### Use the @Value annotation
```java
	@Value("${coach.name}")
	private String coachName;
	
	@Value("${team.name}")
	private String teamName;
```

## Configure the Spring Boot Server
* More info at: https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html
* Changes on the application.properties file
### Change the Spring Boot Server Port
```
server.port=7070
```

### Set the context path of the application
```
server.servlet.context-path=/mycoolapp
```