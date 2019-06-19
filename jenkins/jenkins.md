# Jenkins Notes

##  Introduction and Getting Started
Reference: https://www.youtube.com/watch?v=89yWXXIOisk

### Step 1 : Download Jenkins war file - https://jenkins.io/

### Step 2 : Place the war file into any location on your system

## Step 3 : goto command prompt (windows) | terminal (mac)
* goto folder where jenkins.war is
```
java -jar jenkins.war
```
* Jenkins is installed at: C:\Users\jcabelloc\.jenkins

### Step 4 : goto browser - http://localhost:8080 (Jenkins window should show up)
* check the password in the log: df5...c66
* This may also be found at:  C:\Users\jcabelloc\.jenkins\secrets\initialAdminPassword

### Step 5 : install required plugins
* Pick the choice: Select Plugins to Install / Install Suggested Plugins

### Step 6 : get started with Jenkins 
* Continue as Admin
* Jenkins URL: http://localhost:8080/


## How to setup Jenkins on Tomcat
Reference: https://www.youtube.com/watch?v=Fi9pgrMlQZY
**SKIPPED**


## How to change Home Directory
Reference: https://www.youtube.com/watch?v=m47MWSXNslg

### Step 1 : Check your current home directory
* Go to: Manage Jenkins / Configure System

### Step 2 : Create a new folder (which will be new home dir)
* C:\jcabelloc\Programs\jenkins_home

###  Step 3 : Copy (Move) all data from old dir to new dir
* C:\Users\jcabelloc\.jenkins -> C:\jcabelloc\Programs\jenkins_home

### Step 4 : change env variable - JENKINS_HOME and set to new dir
* Windows - change env variable. Environment Variables / System Variables
JENKINS_HOME=C:\jcabelloc\Programs\jenkins_home

### Step 5 : restart Jenkins
* From C:\jcabelloc\Installers\jenkins:
```
java -jar jenkins.war
```

### Step 6 : Check your current home directory
* Go to: Manage Jenkins / Configure System


 ## How to use CLI - command line interface
Reference: https://www.youtube.com/watch?v=ooA8RS3hC6k&t=4s

### Step 1 : start Jenkins
* From C:\jcabelloc\Installers\jenkins:
```
java -jar jenkins.war
```

### Step 2 : goto Manage Jenkins - Configure Global Security - enable security
* Check that "enable security" is checked

### Step 3 : goto - http://localhost:8080/cli/

### Step 4 : download jerkins-cli jar. Place at any location.
* Placed at: C:\jcabelloc\Installers\jenkins

### Step 5 : test the jenkins command line is working
* go to: C:\jcabelloc\Installers\jenkins
```
java -jar jenkins-cli.jar -s http://localhost:8080/ help --username=admin --password=df....c66
```

### Step 6: Manage Settings / Configure Global Security / Anyone can do anything
```
java -jar jenkins-cli.jar -s http://localhost:8080/ list-plugins
```

## How to create Users + Manage + Assign Roles
Reference: https://www.youtube.com/watch?v=QvFungzXI5s
Jenkins authentication and authorization

* How to create New Users
* How to configure users
* How to create new roles
* How to assign users to roles
* How to Control user access on projects

### Step 1 : Create new users
* Manage Jenkings / Manage Users / Create User

### Step 2 : Configure users
* User icon / configure

### Step 3 : Create and manage user roles Roles Strategy Plugin - download - restart jenkins
* Go to Manage Jenkins / Manage plugins / Available / Search: Role-based Authorization Strategy
* Restart jenkins

### Step 4 : Manage Jenkins - Configure Global Security - Authorization - Role Based Strategy
* Check: Role-Based Strategy

### Step 5 : Create Roles and Assign roles to users
* Go to Manage Jenkins / Manage and Assign Roles

### Step 6 : Validate authorization and authentication are working properly


## Basic Configurations
Reference: https://www.youtube.com/watch?v=Cr8XSljgEPI

### Step 1 : Goto Manage Jenkins - Configure System

### Step 2 : Get a basic understanding of common/basic configurations