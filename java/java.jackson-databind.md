# Jackson Databind Notes

### Add Maven Dependency
```xml
    <dependency>
	    <groupId>com.fasterxml.jackson.core</groupId>
	    <artifactId>jackson-databind</artifactId>
	    <version>2.9.6</version>
	</dependency>
```

### JSON to Java POJO

JSON object Demo
```json
{
	"id": 14,
	"firstName": "Jose",
	"lastName": "Perez",
	"active": true
}
```
Java POJO Class
```java
public class Student {
	private int id;
	private String firstName;
	private String lastName;
	private boolean active;
	
	// getters and setters
```

JSON to Java Example
```java
    //...
    ObjectMapper mapper = new ObjectMapper();
        	
    Student myStudent = mapper.readValue(new File("data/sample-lite.json"), Student.class);
    
    System.out.println("First Name: " + myStudent.getFirstName());
    System.out.println("Last Name: " + myStudent.getLastName());
    //...
```

### Ignore Unknown properties
* To ignore some property from the JSON object, annotate the class
* New JSON object has an additional property
```json
{
	"id": 14,
	"firstName": "Jose",
	"lastName": "Perez",
    "active": true,
    "company": "ABC Inc"
}
```
Annotate the Java POJO class
```java
@JsonIgnoreProperties(ignoreUnknown=true)
public class Student { ...} 
```