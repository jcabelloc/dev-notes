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

## Adding Spring Security to an MVC Project

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

## Starting a Basic Spring Security Configuration

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
			.withUser(users.username("jhon").password("123").roles("employee"));
		
		auth.inMemoryAuthentication()
			.withUser(users.username("jack").password("123").roles("manager"));
		
		auth.inMemoryAuthentication()
			.withUser(users.username("cindy").password("123").roles("admin"));
	}
}
```

## Adding a custom login

### Step 1: Modify Spring Security Configuration
```java
// Override a second method in the Security Config Class
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
	//...
	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http.authorizeRequests()
				.anyRequest().authenticated()
			.and()
			.formLogin()
				.loginPage("/loginPage")
				.loginProcessingUrl("/authenticator")
				.permitAll();
	}
}
```

### Step 2: Add a Controller for the Custom Login Form
```java
@Controller
public class LoginController {

	@GetMapping("loginPage")
	public String loginPage() {
		return "login";
	}
}
```

### Step 3: Create a custom Login Form (login.jsp)
```jsp
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<html>
<head>
	<title>Custom Login Page</title>
</head>
<body>
	<h3>Login Page</h3>
	<form:form action="${pageContext.request.contextPath}/authenticator" method="POST">
		
		<p> User name: <input type="text" name="username"></p>
		<p> Password: <input type="password" name="password"></p>
		<input type="submit" value="Login"/>
		
	</form:form>
</body>
</html>
```

## Adding login error messages

### Modify Form - Check for Error
```jsp
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<%@ taglib uri = "http://java.sun.com/jsp/jstl/core" prefix = "c" %>

<html>
<head>
	<title>Custom Login Page</title>
	<style>
		.failed{ color:red;}
	</style>
</head>
<body>
	<h3>Login Page</h3>
	<form:form action="${pageContext.request.contextPath}/authenticator" method="POST">
		
		<c:if test="${param.error !=null}">
			<i class="failed">Invalid username/password</i>
		</c:if>
		
		<p> User name: <input type="text" name="username"></p>
		<p> Password: <input type="password" name="password"></p>
		<input type="submit" value="Login"/>
		
	</form:form>
</body>
</html>
```
## Adding a new login using BootStrap
### Create a new login jsp
```jsp
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>  
<!doctype html>
<html lang="en">
<head>
	<title>Login Page</title>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
	
	<!-- Reference Bootstrap files -->
	<link rel="stylesheet"
		 href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
	
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.0/jquery.min.js"></script>
	
	<script	src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>

</head>
<body>
	<div>
		<div id="loginbox" style="margin-top: 50px;"
			class="mainbox col-md-3 col-md-offset-2 col-sm-6 col-sm-offset-2">
			<div class="panel panel-info">
				<div class="panel-heading">
					<div class="panel-title">Sign In</div>
				</div>
				<div style="padding-top: 30px" class="panel-body">
					<!-- Login Form -->
					<form:form action="${pageContext.request.contextPath}/authenticator" 
							   method="POST" class="form-horizontal">
					    <!-- Place for messages: error, alert etc ... -->
					    <div class="form-group">
					        <div class="col-xs-15">
					            <div>
								<!-- Check for login error -->
								<c:if test="${param.error != null}">
									<div class="alert alert-danger col-xs-offset-1 col-xs-10">
										Invalid username and/or password.
									</div>
								</c:if>
					            </div>
					        </div>
					    </div>
						<!-- User name -->
						<div style="margin-bottom: 25px" class="input-group">
							<span class="input-group-addon"><i class="glyphicon glyphicon-user"></i></span> 
							<input type="text" name="username" placeholder="username" class="form-control">
						</div>
						<!-- Password -->
						<div style="margin-bottom: 25px" class="input-group">
							<span class="input-group-addon"><i class="glyphicon glyphicon-lock"></i></span> 
							<input type="password" name="password" placeholder="password" class="form-control" >
						</div>
						<!-- Login/Submit Button -->
						<div style="margin-top: 10px" class="form-group">						
							<div class="col-sm-6 controls">
								<button type="submit" class="btn btn-success">Login</button>
							</div>
						</div>
					</form:form>
				</div>
			</div>
		</div>
	</div>
</body>
</html>
```
### Modify the controller
```java
Controller
public class LoginController {

	@GetMapping("loginPage")
	public String loginPage() {
		//return "login";
		return "newlogin";
	}
}
```
## Logout Support

### Add logout support in the Spring Security Config
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
	
	//...

	@Override
	protected void configure(HttpSecurity http) throws Exception {
		
		http.authorizeRequests()
				.anyRequest().authenticated()
			.and()
			.formLogin()
				.loginPage("/loginPage")
				.loginProcessingUrl("/authenticator")
				.permitAll()
			.and() // The magic is here
			.logout().permitAll();
		
	}
}

```

### Add Logout Button (home.jsp)
```jsp
		<form:form action="${pageContext.request.contextPath}/logout" method="POST">
			<input type="submit" value="Logout"/>
		</form:form>
```
### Show logged out message (newlogin.jsp)
```jsp
		<!-- Check for logout -->
		<c:if test="${param.logout != null}">
			<div class="alert alert-success col-xs-offset-1 col-xs-10">
				You have been logged out.
			</div>
		</c:if>
```

## Cross Site Request Forgery (CSRF)

### Manually adding CSRF
First chage <form:form> tag by <form> tag
Then add tokens manually inside the form
```jsp
		<!-- Adding tokens manually... -->
		<input type="hidden"
			name="${_csrf.parameterName}"
			value="${_csrf.token}" />

```


## Showing User and Roles

### Step 1: Update POM File for Spring Security JSP Tag Library (pom.xml)
```xml
	<!-- Add Spring Security Taglibs support -->
  	<dependency>
    	<groupId>org.springframework.security</groupId>
    	<artifactId>spring-security-taglibs</artifactId>
    	<version>${springsecurity.version}</version>
	</dependency>
```

### Step 2: Add Spring JSP Tag Library to JSP Page
```jsp
<%@ taglib prefix="security" uri="http://www.springframework.org/security/tags" %>
```
### Step 3: Display User ID and Display User Roles
```jsp
	<p> 
		User: <security:authentication property="principal.username"/>
		<br><br>
		Role(s): <security:authentication property="principal.authorities"/>
		
	</p>
```


## Restricting Access based on Roles

### Step 1: Create Supporting Controller code and View pages
```java
@Controller
public class SampleController {
	//...	
	@GetMapping("/leaders")
	public String showLeaders() {
		return "leaders";
	}
	
	@GetMapping("/systems")
	public String showSystems() {
		return "systems";
	}
}
```
### Step 2: Update User Roles
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

	@Override
	protected void configure(AuthenticationManagerBuilder auth) throws Exception {
		
		@SuppressWarnings("deprecation")
		UserBuilder users = User.withDefaultPasswordEncoder();
		auth.inMemoryAuthentication()
			.withUser(users.username("jhon").password("123").roles("employee"));
		
		auth.inMemoryAuthentication()
			.withUser(users.username("jack").password("123").roles("employee", "manager"));
		
		auth.inMemoryAuthentication()
			.withUser(users.username("cindy").password("123").roles("employee", "admin"));
	}
	//...
}
```

### Step 3: Restricting Access to Roles using antMatchers
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
	//...
	@Override
	protected void configure(HttpSecurity http) throws Exception {
		
		http.authorizeRequests()
			.antMatchers("/").hasRole("employee")
			.antMatchers("/leaders/**").hasRole("manager")
			.antMatchers("/systems/**").hasRole("admin")
			.and()
			.formLogin()
				.loginPage("/loginPage")
				.loginProcessingUrl("/authenticator")
				.permitAll()
			.and()
			.logout().permitAll();
		
	}
}
```



## Configure Custom page access denied

### Add support for an Access Denied Page Handler
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
	//...
	@Override
	protected void configure(HttpSecurity http) throws Exception {
		
		http.authorizeRequests()
			.antMatchers("/").hasRole("employee")
			.antMatchers("/leaders/**").hasRole("manager")
			.antMatchers("/systems/**").hasRole("admin")
			.and()
			.formLogin()
				.loginPage("/loginPage")
				.loginProcessingUrl("/authenticator")
				.permitAll()
			.and()
			.logout().permitAll()
			// THe magic is here
			.and()
			.exceptionHandling().accessDeniedPage("/access-denied");
	}
}
```
### Configure the controller
```java
@Controller
public class LoginController {
	//...	
	@GetMapping("/access-denied")
	public String showAccessDenied() {
		return "access-denied";
	}
}

```

## Show content based on Roles
### Edit the jsp file
```jsp
	<security:authorize access="hasRole('manager')">
		<p>
			<a href="${pageContext.request.contextPath}/leaders">Leadership Option</a>
		</p>		
	</security:authorize>

	<security:authorize access="hasRole('admin')">
		<p>
			<a href="${pageContext.request.contextPath}/systems">Systems Admin</a>
		</p>		
	</security:authorize>
```

