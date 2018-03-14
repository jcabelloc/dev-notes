# Spring Core - Java Annotations

## Inversion of Control

### Basic Java Annotation Configuration

```xml
<!-- In the applicationContext.xml, add entry to enable component scanning -->
	<context:component-scan base-package="edu.tamu.jcabelloc.springdemo"/>
```

```java
// Add @Component Annotation
@Component("theBestProfessor")
public class MathProfessor implements Professor {
	@Override
	public String getTask() {
		return "Practice 20 Arithmetic Problems";
	}
}
```

```java
// Retrieve bean from Container
public static void main(String[] args) {
		ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
		
		Professor myBestProfessor = context.getBean("theBestProfessor", Professor.class);
		
		System.out.println(myBestProfessor.getTask());
		
		context.close();

}
```

### Basic Java Annotation Configuration - Default Name

```java
// Add @Component Annotation - WITHOUT NAME!
@Component
public class MathProfessor implements Professor {
	@Override
	public String getTask() {
		return "Practice 20 Arithmetic Problems";
	}
}
```

```java
// Retrieve bean from Container
public static void main(String[] args) {
		ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
		
		Professor myBestProfessor = context.getBean("mathProfessor", Professor.class);
		
		System.out.println(myBestProfessor.getTask());
		
		context.close();

}
```

## Dependency Injection

### Dependency Injection - Constructor Injection

```java
// Set up interface and class to inject
@Component
public class RegularClassroomService implements ClassroomService {
	@Override
	public String getClassroom() {
		return "Take the 101 room";
	}
}
```

```java
// Create a construction for injection
@Component
public class MathProfessor implements Professor {
	
	ClassroomService classroomService;
	
	public MathProfessor(ClassroomService theClassroomService) {
		this.classroomService = theClassroomService;
	}
	//...
}
```

```java
// Use the @Autowired Annotation for Dependency Injection
@Component
public class MathProfessor implements Professor {
	
	ClassroomService classroomService;
	
	@Autowired
	public MathProfessor(ClassroomService theClassroomService) {
		this.classroomService = theClassroomService;
	}

	@Override
	public String getTask() {
		return "Practice 20 Arithmetic Problems";
	}

	@Override
	public String getClassroom() {
		return classroomService.getClassroom();
	}
}
```

### Dependency Injection - Setter Injection
```java
// Create a setter Methods
// Configure the dependency injection with the @Autowired annotation
@Component
public class PhysicsProfessor implements Professor {
	
	ClassroomService classroomService;
	
	@Override
	public String getTask() {
		return "Study 1st Newtown's Law";
	}
	
	// The magic is here!
	@Autowired
	public void setClassroomService(ClassroomService theClassroomService) {
		this.classroomService = theClassroomService;
	}
	
	@Override
	public String getClassroom() {
		return classroomService.getClassroom();
	}
}
```

```java
	// Use Whatever name you want
	@Autowired
	public void myWhateverMethod(ClassroomService theClassroomService) {
		this.classroomService = theClassroomService;
	}
```

### Dependency Injection - Field Injection
```java
@Component
public class PhysicsProfessor implements Professor {
	
	// The magic is here!
	@Autowired
	ClassroomService classroomService;
	
	@Override
	public String getTask() {
		return "Study 1st Newtown's Law";
	}
	
	@Override
	public String getClassroom() {
		return classroomService.getClassroom();
	}
}
``````

### Dependency Injection - Qualifiers

```java
// We have more than implementation for the interface
@Component
public class MultimediaClassroomService implements ClassroomService {

	@Override
	public String getClassroom() {
		return "Go to 201 room";
	}
}

```

``` java
// We need to use the @Qualifier annotation
@Component
public class PhysicsProfessor implements Professor {
	
	@Autowired
	@Qualifier("multimediaClassroomService")  // The magic is here!
	ClassroomService classroomService;
	
	@Override
	public String getTask() {
		return "Study 1st Newtown's Law";
	}

	@Override
	public String getClassroom() {
		return classroomService.getClassroom();
	}
}
```

```java
// @Qualifier annotation for Constructor Injection
@Component
public class MathProfessor implements Professor {

	ClassroomService classroomService;
	
	@Autowired
	public MathProfessor(@Qualifier("regularClassroomService") ClassroomService theClassroomService) {
		this.classroomService = theClassroomService;
	}

	@Override
	public String getTask() {
		return "Practice 20 Arithmetic Problems";
	}

	@Override
	public String getClassroom() {
		return classroomService.getClassroom();
	}
}
```

### Bean Scopes - @Scope annotation
* Remember that the default @Scope is singleton

```java
// By default spring delivers singleton objects
@Component
@Scope("singleton")
public class MathProfessor implements Professor {

	// ...
}
```

```java
// Use of prototype (a new object for each request)
@Component
@Scope("prototype")
public class MathProfessor implements Professor {

	// ...
}
```
### Bean Lifecycle Methods
* For "prototype" scoped beans, Spring does not call the @PreDestroy method
```java
// Use @PostConstruct and @PreDestroy annotations 
@Component
public class MathProfessor implements Professor {

	@PostConstruct
	public void doAtInitialization() {
		System.out.println("Running Post Construct");
	}
	
	@PreDestroy
	public void doAtDestruction() {
		System.out.println("Running Pre Destroy");
	}
	// ...
}
```