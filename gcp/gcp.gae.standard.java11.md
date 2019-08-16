# GCP / GAE / Standard / Java 11

* Reference: https://cloud.google.com/appengine/docs/standard/java11/quickstart

### Install / update google gcloud
```cmd
gcloud components update
```

### Create a new project
```cmd
gcloud projects create sayri-draft --set-as-default
gcloud projects describe sayri-draft
gcloud app create --project=sayri-draft
```
* Select region: us-central

### Enable billing


### Install App Engine Component (Already done)

```cmd
gcloud components install app-engine-java
```
### Create a Spring project

### Add a maven dependency to pom.xml
```xml
            <plugin>
            <groupId>com.google.cloud.tools</groupId>
            <artifactId>appengine-maven-plugin</artifactId>
            <version>2.1.0</version>
            <configuration>
                <version>springboot-helloworld</version>
            </configuration>
            </plugin>
```
### Creating the app.yaml file
```
runtime: java11
instance_class: F2

# Explicitly set the memory limit and maximum heap size for the Spring Boot app
env_variables:
  JAVA_TOOL_OPTIONS: "-XX:MaxRAM=256m -XX:ActiveProcessorCount=2 -Xmx32m"
```

### Deploy your project

```cmd
mvn clean package appengine:deploy -Dapp.deploy.projectId=YOUR_PROJECT_ID
gcloud app browse
```

## Connect to a Google Cloud MYSQL DB from a local java application using JDBC
* Reference: https://github.com/GoogleCloudPlatform/cloud-sql-jdbc-socket-factory

### Activate credentials
```cmd
gcloud auth application-default login
```

### Add a dependency
```xml
	    <dependency>
		    <groupId>com.google.cloud.sql</groupId>
		    <artifactId>mysql-socket-factory-connector-j-8</artifactId>
		    <version>1.0.14</version>
		</dependency>
```
### Incoportate a JDBC url
* Format: jdbc:mysql://google/<DATABASE_NAME>?cloudSqlInstance=<INSTANCE_CONNECTION_NAME>&socketFactory=com.google.cloud.sql.mysql.SocketFactory&useSSL=false&user=<MYSQL_USER_NAME>&password=<MYSQL_USER_PASSWORD>
* File application.properties
```
spring.datasource.url=jdbc:mysql://google/sayri_desa?useSSL=false&cloudSqlInstance=sayri-draft:us-central1:sayri&socketFactory=com.google.cloud.sql.mysql.SocketFactory
spring.datasource.username=nombre_usuario
spring.datasource.password=clave
spring.jpa.show-sql=true
```
