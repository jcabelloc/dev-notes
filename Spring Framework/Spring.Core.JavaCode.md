# Spring Core - Using Java Code

### Spring Configuration with Java Code

```java
// Create a Java Class and annotate as @Configuration
// Add componenet scanning support @ComponentScan
@Configuration
@ComponentScan("edu.tamu.jcabelloc.springdemo")
public class MyApplicationConfig {

}
```

```java
// Read Spring Java Configuration Class
public class JavaConfigCollegeApp {

	public static void main(String[] args) {
		
		//The magic is here!
		AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(MyApplicationConfig.class);  
		
		Professor myBestProfessor = context.getBean("mathProfessor", Professor.class);
		
		System.out.println(myBestProfessor.getClassroom());

		context.close();

	}

}

```

### Spring Beans with Java Code

```java
// Set up a new Class implementarion to Inject
public class ComputerClassroomService implements ClassroomService {

	@Override
	public String getClassroom() {
		return "Go to Lab with 100 PCs";
	}
}
```
```java
// Set up a new Class to receive the Injection through the constructor
public class ProgrammingProfessor implements Professor {
	
	ClassroomService classroomService;
	
	public ProgrammingProfessor(ClassroomService theClassroomService) {
		this.classroomService = theClassroomService;
	}
	
	@Override
	public String getTask() {
		return "Practice Data Structures";
	}

	@Override
	public String getClassroom() {
		return classroomService.getClassroom();
	}
}
```

```java
// In the Java Configuration Class
// Define methods to expose beans  and // Inject Bean Dependency
@Configuration
public class MyApplicationConfig {
	
	@Bean
	public ClassroomService computerClassroomService() {
		return new ComputerClassroomService();
	} 
	
	@Bean
	public Professor programmingProfessor() {
		return new ProgrammingProfessor(computerClassroomService());
	}
}
```
```java
// Read Spring Java Configuration Class and retrieve the bean
public class JavaConfigCollegeApp {

	public static void main(String[] args) {
		
		AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(MyApplicationConfig.class);  
		
		// that 'programmingProfessor' name must match the method name used to expose the bean
		Professor myBestProfessor = context.getBean("programmingProfessor", Professor.class);
		
		System.out.println(myBestProfessor.getClassroom());

		context.close();
	}
}
```

### Injecting values from a propery file

