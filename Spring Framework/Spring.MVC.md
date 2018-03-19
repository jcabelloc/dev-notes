# Spring MVC Notes




## Set up a reference MVC Project with Pure Java Configuration

### Step 0: Create a maven project with the spring-mvc-archetype

* In Eclipse create a Maven Project
* Select the maven-archetype-webapp

### Step 1: Add Maven Dependencies for Spring MVC Web App
Update the pom.xml file looks that way

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>edu.tamu.jcabelloc</groupId>
  <artifactId>spring-mvc-security</artifactId>
  <packaging>war</packaging>
  <version>0.0.1-SNAPSHOT</version>
  <name>spring-mvc-security Maven Webapp</name>
  <url>http://maven.apache.org</url>
  
  <properties>
   	<springframework.version>5.0.4.RELEASE</springframework.version>
	<maven.compiler.source>1.8</maven.compiler.source>
	<maven.compiler.target>1.8</maven.compiler.target>
  </properties>
  
  <dependencies>
  	<!-- Spring MVC support -->
  	<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
	  <version>${springframework.version}</version>
  	</dependency>
  	
  	<!-- Servlet, JSP and JSTL support -->
  	<dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
	</dependency>
  	
  	<dependency>
	  <groupId>javax.servlet.jsp</groupId>
      <artifactId>javax.servlet.jsp-api</artifactId>
      <version>2.3.1</version>
	</dependency>
  
  	<dependency>
   	  <groupId>javax.servlet</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
	</dependency>
	
  
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <build>
    <finalName>spring-mvc-security</finalName>
    <pluginManagement>
    	<plugins>
            <plugin>
    			<!-- Maven Coordinates (GAV) for maven-war-plugin-->
    			<groupId>org.apache.maven.plugins</groupId>
    			<artifactId>maven-war-plugin</artifactId>
    			<version>3.2.0</version>
         	</plugin>
     	</plugins>
  	</pluginManagement>
  </build>
</project>
```

### Steo 2: Create Spring App Configuration
* Since we are using Pure Java Configuration, delete the web.mxl file located in /WEB-INF/

```java
// Set up the view resolver
@Configuration
@EnableWebMvc
@ComponentScan(basePackages="edu.tamu.jcabelloc.springmvcsecurity")
public class AppConfig {
	
	@Bean
	public ViewResolver viewResolver() {
		InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
		viewResolver.setPrefix("/WEB-INF/view/");
		viewResolver.setSuffix(".jsp");
		return viewResolver;
	}
}
```

### Step 3: Create Spring Dispatcher Servlet Initializer

```java
public class MyWebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

    @Override
    protected Class<?>[] getRootConfigClasses() {
        return null;
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class<?>[] { AppConfig.class };
    }

    @Override
    protected String[] getServletMappings() {
        return new String[] { "/" };
    }
}
```

### Step 4: Develop an Spring Controller
```java
@Controller
public class SampleController {
	
	@GetMapping("/")
	public String showHome() {
		return "home";
	}
}
```

### Step 5: Develop an JSP View page

```jsp
<!-- home.jsp located in /WEB-INF/view/-->
<html>
    <head>
        <title> Home page</title>
    </head>

    <body>
        <h1> Welcome Home Page</h1>
        <hr>
        
    </body>
</html>
```

### Addtional Step - Change the dynamic web module version to 3.1

* Edit the project facet configuration file itself: org.eclipse.wst.common.project.facet.core.xml
* You'll find this file in the .settings directory within the Eclipse project.
* Then Update the Maven Project

```xml
<?xml version="1.0" encoding="UTF-8"?>
<faceted-project>
  <fixed facet="wst.jsdt.web"/>
  <installed facet="jst.web" version="3.1"/>
  <installed facet="wst.jsdt.web" version="1.0"/>
  <installed facet="java" version="1.8"/>
</faceted-project>
```

### Add Maven dependencies for Spring Security 

Check dependencies compatibility in the Maven Central repository to choose the correct version
```xml
  	<!-- Spring Security -->
  	<!-- spring-security-web and spring-security-config -->
  	<dependency>
    	<groupId>org.springframework.security</groupId>
    	<artifactId>spring-security-web</artifactId>
    	<version>5.0.3.RELEASE</version>
	</dependency>
	<dependency>
    	<groupId>org.springframework.security</groupId>
    	<artifactId>spring-security-config</artifactId>
    	<version>5.0.3.RELEASE</version>
	</dependency>
```