# Spring Core

## Inversion of Control and Dependency Injection

### Inversion of Control - XML Configuration
```java
// Set up the Interface and Class
public class MathProfessor implements Professor {
	@Override
	public String getTask() {
		return "Solve 10 arithmetic problems";
	}
}
```

```xml
<!-- Configure the Bean in the applicationContext.xml file  --> 
<!-- By default Singleton is the default Spring Bean Scope  --> 
    <bean id = "myProfessor"
   		class= "com.tamu.jcabelloc.springdemo.MathProfessor">
    </bean>
```

```xml
<!-- Configure the Bean in the applicationContext.xml file  --> 
<!-- Prototype Scope: A new object for each request  --> 
    <bean id = "myProfessor"
   		class= "com.tamu.jcabelloc.springdemo.MathProfessor"
   		scope= "prototype" >
    </bean>
```


```java
// Create the container and retrieve the Bean

	public static void main(String[] args) {
		ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml"); 
		
		Professor myProfessor = context.getBean("myProfessor", Professor.class);
		
		System.out.println(myProfessor.getTask());
		
		context.close();
	}

```

### Dependency Injection - Constructor Injection- XML Configuration

```java
// Set up the Interface and Class
public class RegularClassroomService implements ClassroomService{ 
	@Override
	public String getClassroom() {
		return "Your classromm is 101";
	}
}
```

```java
// Inject the Service in the constructor and call its methods
public class MathProfessor implements Professor {
	
	private ClassroomService classroomService;
	
	public MathProfessor(ClassroomService myClassroomService) {
		classroomService = myClassroomService; 
	}
	
	//...

	@Override
	public String getClassrom() {
		return classroomService.getClassroom();
	}
}
```

```xml
<!-- In the  applicationContext.xml file, configure the new Bean and modify ...--> 
    <bean id = "theClassroom"
   		class= "com.tamu.jcabelloc.springdemo.RegularClassroomService">
    </bean>    

    <bean id = "myProfessor"
   		class= "com.tamu.jcabelloc.springdemo.MathProfessor">
   		<constructor-arg ref="theClassroom"/>
    </bean>  
```


### Dependency Injection - Setter Injection- XML Configuration

```java
// Inject the service in a setter method(s)
public class PhysicsProfessor implements Professor {
	
	// Step 1
	private ClassroomService classroomService;
	
	@Override
	public String getTask() {
		return "Study Newton's Laws";
	}
	
	// Step 2
	public void setClassroomService(ClassroomService classroomService) {
		this.classroomService = classroomService;
	}

	@Override
	public String getClassrom() {
		return classroomService.getClassroom();
	}

}
```

```xml
<!-- In the  applicationContext.xml file, configure the dependency injection --> 
    
    <bean id = "theClassroom"
   		class= "com.tamu.jcabelloc.springdemo.RegularClassroomService">
    </bean>  

<!-- Attention: Property name must have the setter method's name (without "set") -->     
    <bean id = "physicsProfessor"
   		class= "com.tamu.jcabelloc.springdemo.PhysicsProfessor">
   		<property name="classroomService" ref="theClassroom" />
    </bean>  
```

## Bean LifeCycle

### Init and Destroy method configuration
```xml
<!-- Configure the Bean in the applicationContext.xml file  --> 
    <bean id = "myProfessor"
   		class = "com.tamu.jcabelloc.springdemo.MathProfessor"
   		init-method ="doAtInitialization"
   		destroy-method = "doAtDestruction">
    </bean>
```