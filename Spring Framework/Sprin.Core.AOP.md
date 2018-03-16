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

### Ordering Aspects

* Step 1: Separate Advices in different Aspects
* Step 2: Add @Order annotation

```java
// Create a utility class for Expressions
@Aspect
public class UtilAOPExpressions {

	@Pointcut("execution(* edu.tamu.jcabelloc.springaopsample.dao.*.*(..))")
	public void pointcutForDAOPackage() {}
	
	@Pointcut("execution(* edu.tamu.jcabelloc.springaopsample.dao.*.get*(..))")
	public void getters() {}
	
	@Pointcut("execution(* edu.tamu.jcabelloc.springaopsample.dao.*.set*(..))")
	public void setters() {}
	
	@Pointcut("pointcutForDAOPackage() && !(getters() || setters())")
	public void forDAOPackageNoGetterNoSetter() {}

}
```

```java
// Separate Advives in Different Aspects
// Add @Order Annotation... Order(1) how greater priority
@Aspect
@Component
@Order(1)
public class MyAuditAspect {
	
	@Before("edu.tamu.jcabelloc.springaopsample.aspect.UtilAOPExpressions.forDAOPackageNoGetterNoSetter()")
	public void performAuditLogging() {
		System.out.println("\n\n Performing Audit Logging ");
	}
}
```
```java
@Aspect
@Component
@Order(2)
public class MyAnalyticsAspect {

	@Before("edu.tamu.jcabelloc.springaopsample.aspect.UtilAOPExpressions.forDAOPackageNoGetterNoSetter()")
	public void logForAnalytics() {
		System.out.println("\n\n Log For Analytics purposes");
	}
}
```
```java
@Aspect
@Component
@Order(3)
public class LoggingAspect {
	@Before("edu.tamu.jcabelloc.springaopsample.aspect.UtilAOPExpressions.forDAOPackageNoGetterNoSetter()")
	public void beforeMethodsInDAOPackageAdvice() {
		System.out.println("\n\n Executing @Before advice on method");
	}
}
```

### JoinPoints
How to read/access method parameters
* Access and display Method Signature
* Access and display Method Arguments

```java
@Aspect
@Component
public class LoggingAspect {

	@Before("edu.tamu.jcabelloc.springaopsample.aspect.UtilAOPExpressions.forDAOPackageNoGetterNoSetter()")
	public void beforeMethodsInDAOPackageAdvice(JoinPoint myJoinPoint) {
		System.out.println("\n\n Executing @Before advice on method");
		
		//Displaying Method's Signature and Arguments
		MethodSignature signature = (MethodSignature)myJoinPoint.getSignature();
		System.out.println("Method Signature: " + signature);
		
		Object[] args = myJoinPoint.getArgs();
		for (Object anArg: args) {
			System.out.println(anArg);
		}
	}
```

### @AfterReturning Advice
Typical use cases are: Logging, security, transactions, audit logging AND Post-Processing Data

```java
// Add a new method to Advice
@Component
public class BankAccountDAO {
	
	private String accountNumber;
	private BigDecimal balance; 
	
	public void deposit(double amount) {
		System.out.println(getClass() + " Doing deposit to the Bank Account..."); 
	}
	
	public String toString() {
		System.out.println(getClass() + " Doing toString in the Bank Account...");		
		return getClass().toString();
 
	}
	// New Method to Advice by @AfterReturning
	public List<BankAccount> findBankAccounts(){
		List<BankAccount> bankAccounts = new ArrayList<>();
		
		BankAccount bankAccount1 = new BankAccount("123","vip");
		BankAccount bankAccount2 = new BankAccount("234","platinum");
		BankAccount bankAccount3 = new BankAccount("345","gold");
		
		bankAccounts.add(bankAccount1);
		bankAccounts.add(bankAccount2);
		bankAccounts.add(bankAccount3);
		
		return bankAccounts;
	}
	//...
}
```
```java
// New Advice using @AfterReturning
@Aspect
@Component
@Order(3)
public class LoggingAspect {
	
	// The magic is here!
	@AfterReturning(
			pointcut="execution(* edu.tamu.jcabelloc.springaopsample.dao.BankAccountDAO.findBankAccounts(..))",
			returning="resultData")
	public void afterReturningFindBankAccounts(JoinPoint myJoinPoint, List<BankAccount> resultData) {
		
		String method = myJoinPoint.getSignature().toString();
		System.out.println("\n\n----- Executing @AfterReturning on Method: " + method);
		
		System.out.println("\n\n----- The result data is: " + resultData);
	}
	//...
}
```
### @AfterReturning Advice - Modify Result Data before Delivering to the calling program

```java
@Aspect
@Component
@Order(3)
public class LoggingAspect {
	
	@AfterReturning(
			pointcut="execution(* edu.tamu.jcabelloc.springaopsample.dao.BankAccountDAO.findBankAccounts(..))",
			returning="resultData")
	public void afterReturningFindBankAccounts(JoinPoint myJoinPoint, List<BankAccount> resultData) {
		
		String method = myJoinPoint.getSignature().toString();
		System.out.println("\n\n----- Executing @AfterReturning on Method: " + method);
		
		System.out.println("\n\n----- The result data is: " + resultData);
		
		// Modifying the data before delivering to the calling program
		convertProductNamesToUpperCase(resultData);

		System.out.println("\n\n----- The NEW result data is: " + resultData);

	}
	
	private void convertProductNamesToUpperCase(List<BankAccount> resultData) {
		for (BankAccount anAccount: resultData) {
			String productUpperName = anAccount.getProduct().toUpperCase();
			anAccount.setProduct(productUpperName);
		}
	}
	//...
}
```

### @AfterThrowing Advice

Some use cases are: Log the Exception, Auditing on the Exception, Notify DevOps team.
Note: The exception is still propagated to the calling program

```java
// Simulate our DAO class emit an Exception
@Component
public class BankAccountDAO {
	
	private String accountNumber;
	private BigDecimal balance; 
	
	public void deposit(double amount) {
		System.out.println(getClass() + " Doing deposit to the Bank Account..."); 
	}
	
	public String toString() {
		System.out.println(getClass() + " Doing toString in the Bank Account...");		
		return getClass().toString();
 
	}
	// Changes Here!
	public List<BankAccount> findBankAccounts(boolean flagException){
		
		// simulating the exception
		if (flagException) {
			throw new RuntimeException("Simulated Exception");
		}
		
		List<BankAccount> bankAccounts = new ArrayList<>();
		
		BankAccount bankAccount1 = new BankAccount("123","vip");
		BankAccount bankAccount2 = new BankAccount("234","platinum");
		BankAccount bankAccount3 = new BankAccount("345","gold");
		
		bankAccounts.add(bankAccount1);
		bankAccounts.add(bankAccount2);
		bankAccounts.add(bankAccount3);
		
		return bankAccounts;
	} 
	// ...	
}

```
```java
// Add the @AfterThrowing annotation
@Aspect
@Component
public class LoggingAspect {
	
	// The Magic is Here!
	@AfterThrowing(
			pointcut="execution(* edu.tamu.jcabelloc.springaopsample.dao.BankAccountDAO.findBankAccounts(..))",
			throwing="finderException")
	public void afterThrowingFindBankAccounts(JoinPoint myJoinPoint, Throwable finderException) {
		
		String method = myJoinPoint.getSignature().toString();
		
		System.out.println("\n\n----- Executing @AfterThrowing on Method: " + method);
		
		System.out.println("\n\n----- The Advice Exception is: " + finderException);
	}
	//...
}

```

### @After Advice
Common use cases: Log the exception, auditing, some code to run regardles of method outcome
```java
// Adding @After annotation 
@Aspect
@Component
public class LoggingAspect {
	
	// The Magic is here! 
	@After("execution(* edu.tamu.jcabelloc.springaopsample.dao.BankAccountDAO.findBankAccounts(..))")
	public void afterFinallyFindBankAccounts(JoinPoint myJoinPoint) {
		
		String method = myJoinPoint.getSignature().toString();
		System.out.println("\n\n----- Executing @After on Method: " + method);
	}
	//...	
}
```

### @Around Advice
* Runs before and After Method Execution
* Most common use cases: logging, auditing, security, pre-procession and post-processing data
* Also for: Instrumentation and profiling code (How long takes a section of code) and managing exceptions


```java
// Create a class to simulate a Service
@Component
public class ExchangeRateService {
	
	public String getRate() {
		
		try {
			TimeUnit.SECONDS.sleep(3);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		return "3.26";
	}
}
```
```java
// Add an @Around annotation
@Aspect
@Component
public class LoggingAspect {
	
	// The magic is here!
	@Around("execution(* edu.tamu.jcabelloc.springaopsample.service.*.getRate(..))")
	public Object aroundGetRate(ProceedingJoinPoint myProceedingJoinPoint) throws Throwable{
		
		String method = myProceedingJoinPoint.getSignature().toString();
		System.out.println("\n\n----- Executing @Around on Method: " + method);
		
		long start = System.currentTimeMillis();
		
		Object result = myProceedingJoinPoint.proceed();
		
		long finish = System.currentTimeMillis();
		
		long duration = finish - start;
		
		System.out.println("\n\n------------- Duration: " + duration/1000 + " seconds");
		
		return result;
	}
	//...
}
```

### @Around Advice - Handling Exceptions

```java
// Simulate the service throw an Exception
@Component
public class ExchangeRateService {
	
	public String getRate() {
		try {
			TimeUnit.SECONDS.sleep(3);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		return "3.26";
	}
	// Here the change!
	public String getRate(boolean flagException) {
		
		if (flagException) {
			throw new RuntimeException("Service Unavailable!!!");
		}
		return getRate();
	}
}
```

```java
// Then modify the ASpect to Handle de Exception
@Aspect
@Component
public class LoggingAspect {

	@Around("execution(* edu.tamu.jcabelloc.springaopsample.service.*.getRate(..))")
	public Object aroundGetRate(ProceedingJoinPoint myProceedingJoinPoint) throws Throwable{
		
		String method = myProceedingJoinPoint.getSignature().toString();
		System.out.println("\n\n----- Executing @Around on Method: " + method);
		
		long start = System.currentTimeMillis();
		
		Object result = null;
		// Here the change!. Handling and hiding the Exception
		try {
			result = myProceedingJoinPoint.proceed();
		}catch(Exception e) {
			System.out.println(e.getMessage());
			result = "rate: not Available";
		}
		
		long finish = System.currentTimeMillis();
		
		long duration = finish - start;
		
		System.out.println("\n\n------------- Duration: " + duration/1000 + " seconds");
		
		return result;
	}
	//...
}
```
### @Around Advice - Rethrowing the Exception

```java
@Aspect
@Component
public class LoggingAspect {
	
	@Around("execution(* edu.tamu.jcabelloc.springaopsample.service.*.getRate(..))")
	public Object aroundGetRate(ProceedingJoinPoint myProceedingJoinPoint) throws Throwable{
		
		String method = myProceedingJoinPoint.getSignature().toString();
		System.out.println("\n\n----- Executing @Around on Method: " + method);
		
		long start = System.currentTimeMillis();
		
		Object result = null;
		
		try {
			result = myProceedingJoinPoint.proceed();
		}catch(Exception e) {
			System.out.println(e.getMessage());
			throw e;
		}
		
		long finish = System.currentTimeMillis();
		
		long duration = finish - start;
		
		System.out.println("\n\n------------- Duration: " + duration/1000 + " seconds");
		
		return result;
	}
	//...
}
```