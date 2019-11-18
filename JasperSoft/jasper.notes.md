# Jasper Report Notes

### Install JasperSoft Studio
* Download and unzip file: TIB_js-studiocomm_6.10.0_windows_x86_64.zip

### Add mysql jdbc driver
* download or obtain from maven repository: mysql-connector-java-8.0.17.jar
* Add this driver when create a new datasource connection

## Set up java spring boot project
### Add this dependency to the pom.xml file
```xml
    <dependency>
        <groupId>net.sf.jasperreports</groupId>
        <artifactId>jasperreports</artifactId>
        <version>6.9.0</version>
    </dependency>
```

### Make a report available
* Compile the report using JasperSoft Studio
* Place the "report".jasper into src/main/resources

