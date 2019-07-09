# Spring REST Notes

### Initial Notes
* Spring uses the Jackson Project behind the scenes for JSON and Java POJO Data Binding
* In Spring REST Applications, Spring will automatically handle Jackson Integration (JSON -> POJO -> JSON)
* Maven Dependency: com.fasterxwml.jackson.core/jackson-databind

## Spring REST - Development Process

### Step 0: Create a maven project

* In Eclipse create a  Project
* Select the -archetype-webapp
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

### Add Support for JSP
```xml
<!-- Add Support for JSP... get rid of Eclipse Error -->
<dependency>
	<groupId>javax.servlet.jsp</groupId>
	<artifactId>javax.servlet.jsp-api</artifactId>
	<version>2.3.1</version>
</dependency>
```

### Using @PathVariable for REST calls
```java
	//...
	@GetMapping("/students/{studentId}")
	public Student getStudent(@PathVariable int studentId) {
		return students.get(studentId);
	}
	//...
```


## Exception handling

### Create a custom error response class
```java
public class StudentErrorResponse {
	
	private int status;
	private String message;
	private long timeStamp;
	
	public StudentErrorResponse() {
		
	}

	public StudentErrorResponse(int status, String message, long timeStamp) {
		super();
		this.status = status;
		this.message = message;
		this.timeStamp = timeStamp;
	}
	// getters and setters...
```


### Create a custom exception class
```java
public class StudentNotFoundException extends RuntimeException {

	public StudentNotFoundException(String message, Throwable cause) {
		super(message, cause);
	}

	public StudentNotFoundException(String message) {
		super(message);
	}

	public StudentNotFoundException(Throwable cause) {
		super(cause);
	}
}
```

### Update REST service to throw exception if student not found
```java
//...
	@GetMapping("/students/{studentId}")
	public Student getStudent(@PathVariable int studentId) {
		
		if ((studentId >= students.size()) || (studentId < 0)) {
			throw new StudentNotFoundException("Student id not found: " + studentId);
		}
		return students.get(studentId);
	}
//...
```


### Add an exception handler method using @Exception Handler
```java
// In the REST Controller Class
//...
	@ExceptionHandler
	public ResponseEntity<StudentErrorResponse> handleException(StudentNotFoundException exc) {
		
		StudentErrorResponse error = new StudentErrorResponse();
		error.setStatus(HttpStatus.NOT_FOUND.value());
		error.setMessage(exc.getMessage());
		error.setTimeStamp(System.currentTimeMillis());
		return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);
	}

//...
```

### Add a Generic Exception
```java
// In the REST Controller Class
	//...
	// To catch any exception
	@ExceptionHandler
	public ResponseEntity<StudentErrorResponse> handleException(Exception exc) {
		
		StudentErrorResponse error = new StudentErrorResponse();
		error.setStatus(HttpStatus.BAD_REQUEST.value());
		error.setMessage(exc.getMessage());
		error.setTimeStamp(System.currentTimeMillis());
		return new ResponseEntity<>(error, HttpStatus.BAD_REQUEST);
		
	}
```

## Global Exception Handling
* Promote reuse
* Centralize exception handling
* @ControllerAdvice is similar to an interceptor/filter
  * Pre-process requests to controllers
  * Post-process responses to handle exceptions
  * Ideal for global exception handling

### Create new @ControllerAdvice
```java
@ControllerAdvice
public class StudentRestExceptionHandler {
	

}
```

### Refactor REST service (remove old exception handling code)


### Add exception handling code to @ControllerAdvice
```java
@ControllerAdvice
public class StudentRestExceptionHandler {
	
	@ExceptionHandler
	public ResponseEntity<StudentErrorResponse> handleException(StudentNotFoundException exc) {
		
		StudentErrorResponse error = new StudentErrorResponse();
		error.setStatus(HttpStatus.NOT_FOUND.value());
		error.setMessage(exc.getMessage());
		error.setTimeStamp(System.currentTimeMillis());
		return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);
	}
	// To catch any exception
	@ExceptionHandler
	public ResponseEntity<StudentErrorResponse> handleException(Exception exc) {
		
		StudentErrorResponse error = new StudentErrorResponse();
		error.setStatus(HttpStatus.BAD_REQUEST.value());
		error.setMessage(exc.getMessage());
		error.setTimeStamp(System.currentTimeMillis());
		return new ResponseEntity<>(error, HttpStatus.BAD_REQUEST);
		
	}
	
}
```

## Complete CRUD Rest Service

```java
@RestController
@RequestMapping("/api")
public class CustomerRestController {

	@Autowired
	CustomerService customerService;
	
	@GetMapping("/customers")
	public List<Customer> getCustomers() {
		return customerService.getCustomers();
	}
	
	@GetMapping("/customers/{customerId}")
	public Customer getCustomer(@PathVariable int customerId) {
		Customer customer = customerService.getCustomer(customerId);
		if (customer == null) {
			throw new CustomerNotFoundException("Customer id not found - " + customerId);
		}
		return customer;
	}
	
	@PostMapping("/customers")
	public Customer addCustomer(@RequestBody Customer customer) {
		// Just in case the pass an id in JSON, set id to 0
		// this is force a save of new item... instead of update
		customer.setId(0);
		
		customerService.saveCustomer(customer);
		return customer;
	}
	@PutMapping("/customers")
	public Customer updateCustomer(@RequestBody Customer customer) {
		customerService.saveCustomer(customer);
		return customer;
	}
	
	@DeleteMapping("/customers/{customerId}")
	public String deleteCustomer(@PathVariable int customerId) {
		
		Customer customer = customerService.getCustomer(customerId);
		if (customer == null) {
			throw new CustomerNotFoundException("Customer id not found - " + customerId);
		}
		
		customerService.deleteCustomer(customerId);
		return "Delete customer id - " + customerId;
		
	}
	
}
```

