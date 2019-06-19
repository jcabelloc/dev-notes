# Building a Spring Boot Project - Rest CRUD API using Hibernate
* Notes from Chad Darby Course

### Set up the database
* MySQL

### Create the Project using Spring initilzr
* Go to: https://start.spring.io/
* Pick up: Web, JPA, DevTools, MySQL
* Import the downloaded project from Eclipse

### Configure the Spring Boot DataSource
In the application.properties file
```
spring.datasource.url=jdbc:mysql://localhost:3306/employee_directory
spring.datasource.username=usuario
spring.datasource.password=clave
```
### Create the Employee Entity
As usual

### DAO Interface and DAO Implementation
```java
public interface EmployeeDAO {
	
	public List<Employee> findAll();
	
	public Employee findById(int id);
	
	public void save(Employee employee);
	
	public void deleteById(int id);
}
```

```java
import javax.persistence.EntityManager;

import org.hibernate.Session;
import org.hibernate.query.Query;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import com.example.cruddemo.entity.Employee;

@Repository
public class EmployeeDAOHibernateImpl implements EmployeeDAO {

	// Define field for EntityManager
	@Autowired
	private EntityManager entityManager;
	
	/* We can also use constructor injection
	@Autowired
	public EmployeeDAOHibernateImpl(EntityManager entityManager) {
		this.entityManager = entityManager;
	}
	*/
	
	@Override
	public List<Employee> findAll() {
		// Get the current hibernate session
		Session currentSession = entityManager.unwrap(Session.class);
		
		// Create query, execute and return
		Query<Employee> query = currentSession
				.createQuery("from Employee", Employee.class);
		
		List<Employee> employees = query.getResultList();
		return employees;
	}

	@Override
	public Employee findById(int id) {
		Session currentSession = entityManager.unwrap(Session.class);
		return currentSession.get(Employee.class, id);
	}

	@Override
	public void save(Employee employee) {
		Session currentSession = entityManager.unwrap(Session.class);
		currentSession.saveOrUpdate(employee);
	}

	@Override
	public void deleteById(int id) {
		Session currentSession = entityManager.unwrap(Session.class);
		Query query = currentSession
			.createQuery("delete from Employee where id=:employeeId");
		query.setParameter("employeeId", id);
		query.executeUpdate();
	}

}

```
### Service Interface and Service Implementation
```java
public interface EmployeeService {

	public List<Employee> findAll();
	
	public Employee findById(int id);
	
	public void save(Employee employee);
	
	public void deleteById(int id);
}
```

```java

@Service
public class EmployeeServiceImpl implements EmployeeService {
	
	@Autowired
	private EmployeeDAO employeeDAO;

	// We can also use the constructor injection	
	/*
	public EmployeeServiceImpl(EmployeeDAO employeeDAO) {
		this.employeeDAO = employeeDAO;
	}
	*/
	
	
	@Override
	@Transactional
	public List<Employee> findAll() {
		return employeeDAO.findAll();
	}

	@Override
	@Transactional
	public Employee findById(int id) {
		return employeeDAO.findById(id);
	}

	@Override
	@Transactional
	public void save(Employee employee) {
		employeeDAO.save(employee);
	}

	@Override
	@Transactional
	public void deleteById(int id) {
		employeeDAO.deleteById(id);
		
	}

}
```

### Creatint REST Controller Methods
```java

@RestController
@RequestMapping("/api")
public class EmployeeRestController {
	
	@Autowired
	private EmployeeService employeeService;
	
	/* We can instead use constructor injection
	@Autowired
	public EmployeeRestController(EmployeeService employeeService) {
		this.EmployeeService = employeeService;
	}
	*/
	// expose "/employees"
	@GetMapping("/employees")
	public List<Employee> findAll(){
		return employeeService.findAll();
	}
	
	// add mapping to GET a single employee
	@GetMapping("/employees/{employeeId}")
	public Employee getEmployee(@PathVariable int employeeId){
		Employee employee =  employeeService.findById(employeeId);
		if (employee == null) {
			throw new RuntimeException("Employee id not found - " + employeeId);
		}
		return employee;
	}
	
	// add mapping for POST
	@PostMapping("/employees")
	public Employee addEmployee(@RequestBody Employee employee) {

		// this is to force a save of new item... instead of update
		employee.setId(0);
		employeeService.save(employee);
		return employee;
	}
	
	// Add mapping for PUT
	@PutMapping("/employees")
	public Employee updateEmployee(@RequestBody Employee employee) {
		employeeService.save(employee);
		return employee;
	}
	
	// Add mapping for Delete
	@DeleteMapping("/employees/{employeeId}")
	public String deleteEmployee(@PathVariable int employeeId) {
		Employee employee = employeeService.findById(employeeId);
		
		if (employee == null) {
			throw new RuntimeException("Employee id not found - " + employeeId);
		}
		
		employeeService.deleteById(employeeId);
		return "Deleted employee id - " + employeeId;
	}
	
}


```

# Building a Spring Boot Project - Rest CRUD API using JPA


### DAO Implementation

```java

@Repository
public class EmployeeDAOJpaImpl implements EmployeeDAO {
	
	@Autowired
	private EntityManager entityManager;
	
	/* We can also use constructor injection
	@Autowired
	public EmployeeDAOJpaImpl(EntityManager entityManager) {
		this.entityManager = entityManager;
	}
	*/
	
	@Override
	public List<Employee> findAll() {
		Query query = entityManager.createQuery("from Employee");
		System.out.println("EmployeeDAOJpaImpl");
		
		List<Employee> employees = query.getResultList();
		
		return employees;
	}

	@Override
	public Employee findById(int id) {
		Employee employee = entityManager.find(Employee.class, id);
		
		return employee;
	}

	@Override
	public void save(Employee employee) {
		Employee dbEmployee = entityManager.merge(employee);
		employee.setId(dbEmployee.getId());
	}

	@Override
	public void deleteById(int id) {
		Query query = entityManager
				.createQuery("delete from Employee where id=: employeeId");
		query.setParameter("employeeId", id);
		query.executeUpdate();

	}

}

```
### Update the Service Implementation
Note: Use @Qualifier in the service

```java
@Service
public class EmployeeServiceImpl implements EmployeeService {
	
	@Autowired
	@Qualifier("employeeDAOJpaImpl")
	private EmployeeDAO employeeDAO;
	...
```

# Building a Spring Boot Project - Rest CRUD API using Spring Data JPA

### Repository Interface
```java
public interface EmployeeRepository extends JpaRepository<Employee, Integer> {
	// no need to write any code
}
```

### Update the Service Implementation
Note: No need to use @Transactional


```java
@Service
public class EmployeeServiceImpl implements EmployeeService {
	
	@Autowired
	private EmployeeRepository employeeRepository;
	
	@Override
	public List<Employee> findAll() {
		return employeeRepository.findAll();
	}
	...
```

# Building a Spring Boot Project - Rest CRUD API using Spring Data REST
* Delete Service Package
* Delete REST package

### Add the SPRING DATA REST dependency
In the pom.xml file
```xml
		<!-- Add dependency for Spring Data REST -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-rest</artifactId>
		</dependency>
		
```

### Custom the API path
In the application.properties file
```
	spring.data.rest.base-path=/magic-api
```

### Configuration, Pagination and Sorting
* Configuration
```java
@RepositoryRestResource(path="members")
public interface EmployeeRepository extends JpaRepository<Employee, Integer> {
	// no need to write any code
}

```

* Pagination (application.properties file)
```
spring.data.rest.default-page-size=20
```

* Sorting (From postman)
http://localhost:8080/magic-api/employees?sort=lastName,desc