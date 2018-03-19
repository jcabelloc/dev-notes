# Spring Security Notes

* Declarative Security
* Programmatic Security

### Login Methods
* HTTP Basic Authentication
* Spring Default Form
* Custom Form

### Authentication and Authorization
* In-Memory
* JDBC
* LDAP
* Custom
* Others...

### Add Maven dependencies for Spring Security to your Spring MVC Project

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

## Starting a Basic Spring Security

### Create Spring Security Initializer

```java
public class SecurityWebApplicationInitializer extends AbstractSecurityWebApplicationInitializer {

}
```

### Create Spring Security Configuration
### Add Users, passwords and roles
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

	@Override
	protected void configure(AuthenticationManagerBuilder auth) throws Exception {
		
		@SuppressWarnings("deprecation")
		UserBuilder users = User.withDefaultPasswordEncoder();
		auth.inMemoryAuthentication()
			.withUser(users.username("jhon").password("123").roles("employee"))
			.withUser(users.username("jack").password("123").roles("manager"))
			.withUser(users.username("cindy").password("123").roles("admin"));
	}
}
```



