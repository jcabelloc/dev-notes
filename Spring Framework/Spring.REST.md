# Spring REST Notes

### Initial Notes
* Spring uses the Jackson Project behind the scenes for JSON and Java POJO Data Binding
* In Spring REST Applications, Spring will automatically handle Jackson Integration (JSON -> POJO -> JSON)
* Maven Dependency: com.fasterxwml.jackson.core/jackson-databind

## Spring REST - Development Process

### Step 0: Create a maven project

* In Eclipse create a Maven Project
* Select the maven-archetype-webapp
  * GroupID: edu.tamu.jcabelloc
  * ArtifactID: spring-rest-demo
* Delete All Content inside src/main/webapp
* To prevent this issue "web.xml is missing and failonmissingwebxml is set to true", follow the next step
* Add the following plugin in the pom.xml file <build> section
```xml
    <plugins>
	     <plugin>
	         <groupId>org.apache.maven.plugins</groupId>
	         <artifactId>maven-war-plugin</artifactId>
	         <version>3.2.0</version>
	     </plugin>
	 </plugins>
```

### Add Maven Dependency for Spring MVC and Jackson Project

* Add Spring-WebMVC dependency to the pom.xml file
```xml
<!-- Spring MVC support and REST Support -->
  	<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
	  <version>${springframework.version}</version>
  	</dependency>
```
* Add Jackson for JSON Conversion
```xml
    <dependency>
	    <groupId>com.fasterxml.jackson.core</groupId>
	    <artifactId>jackson-databind</artifactId>
	    <version>2.9.6</version>
	</dependency>
```
* Add Servlet Support
```xml
  	<dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
	</dependency>
```

### Add Code for All Java Config: @Configuration

```java
@Configuration
@EnableWebMvc
@ComponentScan("edu.tamu.jcabelloc.springrestdemo")
public class AppConfig {
	
}
```

### Add Code for All Java Config: Servlet Initializer
```java
public class SpringMvcDispatcherServletInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

	@Override
	protected Class<?>[] getRootConfigClasses() {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	protected Class<?>[] getServletConfigClasses() {
		return new Class[] {AppConfig.class};
	}

	@Override
	protected String[] getServletMappings() {
		return new String[] { "/" };
	}

}

```


### Create Spring REST Service using @RestController
```java
@RestController
@RequestMapping("/test")
public class FirstRestController {
	
	@GetMapping("/hello")
	public String ssayHello() {
		return "Hello World!!!!";
	}

}

```

### Addtional Step - Change the dynamic web module version to 3.1

* Edit the project facet configuration file itself: org.eclipse.wst.common.project.facet.core.xml
* You'll find this file in the .settings directory within the Eclipse project.
* Then Update the Maven Project
```xml
<?xml version="1.0" encoding="UTF-8"?>
<faceted-project>
  <fixed facet="wst.jsdt.web"/>
  <installed facet="java" version="1.5"/>
  <installed facet="jst.web" version="3.1"/>
  <installed facet="wst.jsdt.web" version="1.0"/>
</faceted-project>

```

### Tell Maven to use Java 8
* Reference: https://maven.apache.org/plugins/maven-compiler-plugin/examples/set-compiler-source-and-target.html
* In the pom.xml file include
```xml
<project>
  [...]
  <properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
  [...]
</project>
```

