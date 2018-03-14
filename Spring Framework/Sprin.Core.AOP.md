# Spring - Core - AOP

## Introduction

### Concepts (from docs.spring.io)

* Aspect: a modularization of a concern that cuts across multiple classes.

* Join point: a point during the execution of a program. In Spring AOP, it is a method execution.

* Advice: action taken by an aspect at a particular join point. 

* Pointcut: a predicate that matches join points. Advice is associated with a pointcut expression and runs at any join point matched by the pointcut

* Target object: object being advised by one or more aspects. 

* AOP proxy: an object created by the AOP framework in order to implement the aspect contracts

* Weaving: linking aspects with other application types or objects to create an advised object. Spring AOP performs weaving at runtime.


## Types of Advice (from docs.spring.io)

* Before advice: Advice that executes before a join point

* After returning advice: Advice to be executed after a join point completes normally

* After throwing advice: Advice to be executed if a method exits by throwing an exception.

* After (finally) advice: Advice to be executed after normal or exceptional return.

* Around advice: Advice that surrounds a join point such as a method invocation. This is the most powerful kind of advice. 

### Set up Environment

* After setting the spring framework project, it is required to add the library: aspectjweaver-1.8.13.jar

### Before Advice

* Most common uses: logging, security, transactions
* Other uses: Audit logging, api management, analytics

```java 
// Create Target Object
@Component
public class BankAccountDAO {
	
	public void deposit(double amount) {
		System.out.println(getClass() + "Doing deposit to the account..."); 
	}
}
```

```java
// Create Spring Java Config Class. Add the @EnableAspectJAutoProxy Annotation
@Configuration
@EnableAspectJAutoProxy
@ComponentScan("edu.tamu.jcabelloc")
public class SampleConfig {

}
```

```java
// Create the app
public static void main(String[] args) {
	
	AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(SampleConfig.class);
	
	BankAccountDAO myBankAccount = context.getBean("bankAccountDAO", BankAccountDAO.class);
	
	myBankAccount.deposit(100.0);
	
	context.close();
}

```

```java
// Create an Aspect (@Aspect) with @Before advice
@Aspect
@Component
public class LoggingAspect {
	// It matches any class with the deposit method
	@Before("execution(public void deposit(double))")
	public void beforeDeposit() {
		System.out.println("\n=====>>> Executing @Before advice on method");
	}
}
```

### Pointcut expressions

General Expression
```
execution(modifiers-pattern? return-type-pattern declaring-type-pattern? method-name-pathern(param-pattern) throws-pattern?)
```


```java
// In case we want a match for an specific class
@Aspect
@Component
public class LoggingAspect {
	
	@Before("execution(public void edu.tamu.jcabelloc.springaopsample.dao.BankAccountDAO.deposit(double))")
	public void beforeDeposit() {
		System.out.println("\n\n Executing @Before advice on method");
	}
}
```

```java
// Using Wildcards for matching any method dep*
@Aspect
@Component
public class LoggingAspect {
	
	@Before("execution(public void dep*(double))")
	public void beforeDeposit() {
		System.out.println("\n\n Executing @Before advice on method");
	}
}
```
```java
// Using Wildcards for matching any return type (modifier-patter is opcional)
@Aspect
@Component
public class LoggingAspect {
	
	@Before("execution(* dep*(double))")
	public void beforeDeposit() {
		System.out.println("\n\n Executing @Before advice on method");
	}
}
```

```java
// Math a specific parameter type. Use full qualified name. 
@Aspect
@Component
public class LoggingAspect {
	
	@Before("execution(* dep*(java.math.BigDecimal))")
	public void beforeDeposit() {
		System.out.println("\n\n Executing @Before advice on method");
	}
}
```

``` java
// Matching any number of parameters
@Aspect
@Component
public class LoggingAspect {
	
	@Before("execution(* dep*(java.math.BigDecimal, ..))") // or  @Before("execution(* dep*(..))") 
	public void beforeDeposit() {
		System.out.println("\n\n Executing @Before advice on method");
	}
}
```
```java
// Matching any method in a specific package
@Aspect
@Component
public class LoggingAspect {
	
	@Before("execution(* edu.tamu.jcabelloc.springaopsample.dao.*.*(..))")
	public void beforeDeposit() {
		System.out.println("\n\n Executing @Before advice on method");
	}
}

```

### Pointcut declarations

```java
// Declare a @Pointcut and reuse that in different advices
@Aspect
@Component
public class LoggingAspect {
	// Creating a pointcut declaration
	@Pointcut("execution(* edu.tamu.jcabelloc.springaopsample.dao.*.*(..))")
	private void pointcutForDAOPackage() {}
	
	// Using the pointcut declaration
	@Before("pointcutForDAOPackage()")
	public void beforeMethodsInDAOPackageAdvice() {
		System.out.println("\n\n Executing @Before advice on method");
	}

	// Using the pointcut declaration
	@Before("pointcutForDAOPackage()")
	public void performAuditLogging() {
		System.out.println("\n\n Performing Audit Logging ");
	}
}
```

### Combining Pointcut Expressions

```java
@Aspect
@Component
public class LoggingAspect {
	
	@Pointcut("execution(* edu.tamu.jcabelloc.springaopsample.dao.*.*(..))")
	private void pointcutForDAOPackage() {}
	
	@Pointcut("execution(* edu.tamu.jcabelloc.springaopsample.dao.*.get*(..))")
	private void getters() {}
	
	@Pointcut("execution(* edu.tamu.jcabelloc.springaopsample.dao.*.set*(..))")
	private void setters() {}
	
	@Pointcut("pointcutForDAOPackage() && !(getters() || setters())")
	private void forDAOPackageNoGetterNoSetter() {}
	
	@Before("forDAOPackageNoGetterNoSetter()")
	public void beforeMethodsInDAOPackageAdvice() {
		System.out.println("\n\n Executing @Before advice on method");
	}
	
	@Before("forDAOPackageNoGetterNoSetter()")
	public void performAuditLogging() {
		System.out.println("\n\n Performing Audit Logging ");
	}
}

```