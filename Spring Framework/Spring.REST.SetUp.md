# Spring REST Set Up Notes

### Create a maven project

* In Eclipse create a Maven Project
* Select the maven-archetype-webapp
  * GroupID: edu.tamu.jcabelloc
  * ArtifactID: spring-fullrestapp-demo
* Delete All Directory src/main/webapp/WEB-INF
* To prevent this issue "web.xml is missing and failonmissingwebxml is set to true", follow the next step
* Add the following plugin in the pom.xml file <build> section
```xml
    <plugins>
    <!-- Builds a Web Application Archive (WAR) file from the project output 
				and its dependencies. -->
	     <plugin>
	         <groupId>org.apache.maven.plugins</groupId>
	         <artifactId>maven-war-plugin</artifactId>
	         <version>3.2.0</version>
	     </plugin>
	 </plugins>
```

### Add Maven Dependencies for Spring MVC, Jackson Project and so on

```xml
  <properties>
	<springframework.version>5.0.6.RELEASE</springframework.version>
	<hibernate.version>5.2.16.Final</hibernate.version>
	<mysql.connector.version>5.1.45</mysql.connector.version>
	<c3po.version>0.9.5.2</c3po.version>

	<maven.compiler.source>1.8</maven.compiler.source>
	<maven.compiler.target>1.8</maven.compiler.target>
  </properties>
  <dependencies>
    <!-- Spring -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${springframework.version}</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>${springframework.version}</version>
    </dependency>
    
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-orm</artifactId>
        <version>${springframework.version}</version>
    </dependency>

    <!-- Add Jackson for JSON converters -->
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.9.5</version>
    </dependency>

    <!-- Hibernate -->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-core</artifactId>
        <version>${hibernate.version}</version>
    </dependency>

    <!-- MySQL -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>${mysql.connector.version}</version>
    </dependency>

    <!-- C3PO -->
    <dependency>
        <groupId>com.mchange</groupId>
        <artifactId>c3p0</artifactId>
        <version>${c3po.version}</version>
    </dependency>

    <!-- Servlet+JSP+JSTL -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
    </dependency>

    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>javax.servlet.jsp-api</artifactId>
        <version>2.3.1</version>
    </dependency>
    <!-- Not required -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>

    <!-- to compensate for java 9 not including jaxb -->
    <dependency>
        <groupId>javax.xml.bind</groupId>
        <artifactId>jaxb-api</artifactId>
        <version>2.3.0</version>
    </dependency>
  </dependencies>
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




### Add Code for All Java Config: @Configuration

```java
@Configuration
@EnableWebMvc
@EnableTransactionManagement
@ComponentScan("edu.tamu.jcabelloc.springfullrestappdemo")
@PropertySource({ "classpath:persistence-mysql.properties" })
public class AppConfig implements WebMvcConfigurer {

	@Autowired
	private Environment env;
	
	private Logger logger = Logger.getLogger(getClass().getName());
	
	@Bean
	public DataSource myDataSource() {
		
		// create connection pool
		ComboPooledDataSource myDataSource = new ComboPooledDataSource();

		// set the jdbc driver
		try {
			myDataSource.setDriverClass("com.mysql.jdbc.Driver");		
		}
		catch (PropertyVetoException exc) {
			throw new RuntimeException(exc);
		}
		
		// for sanity's sake, let's log url and user ... just to make sure we are reading the data
		logger.info("jdbc.url=" + env.getProperty("jdbc.url"));
		logger.info("jdbc.user=" + env.getProperty("jdbc.user"));
		
		// set database connection props
		myDataSource.setJdbcUrl(env.getProperty("jdbc.url"));
		myDataSource.setUser(env.getProperty("jdbc.user"));
		myDataSource.setPassword(env.getProperty("jdbc.password"));
		
		// set connection pool props
		myDataSource.setInitialPoolSize(getIntProperty("connection.pool.initialPoolSize"));
		myDataSource.setMinPoolSize(getIntProperty("connection.pool.minPoolSize"));
		myDataSource.setMaxPoolSize(getIntProperty("connection.pool.maxPoolSize"));		
		myDataSource.setMaxIdleTime(getIntProperty("connection.pool.maxIdleTime"));

		return myDataSource;
	}
	
	private Properties getHibernateProperties() {

		// set hibernate properties
		Properties props = new Properties();

		props.setProperty("hibernate.dialect", env.getProperty("hibernate.dialect"));
		props.setProperty("hibernate.show_sql", env.getProperty("hibernate.show_sql"));
		
		return props;				
	}

	
	// need a helper method 
	// read environment property and convert to int
	
	private int getIntProperty(String propName) {
		
		String propVal = env.getProperty(propName);
		
		// now convert to int
		int intPropVal = Integer.parseInt(propVal);
		
		return intPropVal;
	}	
	
	@Bean
	public LocalSessionFactoryBean sessionFactory(){
		
		// create session factorys
		LocalSessionFactoryBean sessionFactory = new LocalSessionFactoryBean();
		
		// set the properties
		sessionFactory.setDataSource(myDataSource());
		sessionFactory.setPackagesToScan(env.getProperty("hibernate.packagesToScan"));
		sessionFactory.setHibernateProperties(getHibernateProperties());
		
		return sessionFactory;
	}
	
	@Bean
	@Autowired
	public HibernateTransactionManager transactionManager(SessionFactory sessionFactory) {
		
		// setup transaction manager based on session factory
		HibernateTransactionManager txManager = new HibernateTransactionManager();
		txManager.setSessionFactory(sessionFactory);

		return txManager;
	}	
	
}
```

### Add Code for All Java Config: Servlet Initializer
```java
public class MySpringMvcDispatcherServletInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

	@Override
	protected Class<?>[] getRootConfigClasses() {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	protected Class<?>[] getServletConfigClasses() {
		return new Class[] { AppConfig.class };
	}

	@Override
	protected String[] getServletMappings() {
		return new String[] { "/" };
	}
}

```

### Add persistence-mysql.properties file at src/main/resources
```
#
# JDBC connection properties
#
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/web_customer_tracker?useSSL=false
jdbc.user=***********
jdbc.password=*************

#
# Connection pool properties
#
connection.pool.initialPoolSize=5
connection.pool.minPoolSize=5
connection.pool.maxPoolSize=20
connection.pool.maxIdleTime=3000

#
# Hibernate properties
#
hibernate.dialect=org.hibernate.dialect.MySQLDialect
hibernate.show_sql=true
hibernate.packagesToScan=edu.tamu.jcabelloc.springfullrestappdemo.entity
```