# Maven Notes

## 


### Resources
* Maven Reference Manual
http://maven.apache.org/guides/

* Maven Cheat Sheet
https://zeroturnaround.com/rebellabs/maven-cheat-sheet/

* Central Repositories (How to find dependencies)
Official: https://search.maven.org/ 
Additional: https://mvnrepository.com/


### Maven - Eclipse Integration
Check tools are installed:

* m2e- Maven integration in Eclipse. Launching Maven from Eclipse, dependency management and search.

* m2e-wtp- Maven integration for WTP. Project configurators for Eclipse WTP.

### JDK 1.8 compatibility

Modify the pom.xml file
```xml
    <maven.compiler.target>1.8</maven.compiler.target>
    <maven.compiler.source>1.8</maven.compiler.source>
```

### For Maven Web Projects
Modify the pom.xml file
```xml
<!-- we need to add the Servlet API dependency: javax.servlet-api -->
  	<dependency>
	    <groupId>javax.servlet</groupId>
	    <artifactId>javax.servlet-api</artifactId>
	    <version>3.1.0</version>
	</dependency>
```

### Local and Central Maven Repository
* Local: C:\Users\jcabelloc\.m2\repository
* Central: http://repo.maven.apache.org/maven2/


### Configure Additional Repositories

* Pick up a library (i.e. atlassian-mail)
* Search this in: https://mvnrepository.com
* Obtain the dependency and url from that site
* Modify the pom.xml file
```xml
    <dependencies>
        <!-- Add support for Atlassian main: atlassian-mail -->
        <dependency>
            <groupId>com.atlassian.mail</groupId>
            <artifactId>atlassian-mail</artifactId>
            <version>4.0.4</version>
        </dependency>
    </dependencies>
    <repositories>
        <repository>
            <id>atlassian</id>
            <name>the atlassian repo</name>
            <url>https://maven.atlassian.com/content/repositories/atlassian-public/</url>
        </repository>
  </repositories>
```

### Private repository
* https://www.sonatype.com/products-overview
* https://packagecloud.io/
* https://mymavenrepo.com/



