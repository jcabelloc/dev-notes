# Quickstart for Java 8 for App Engine Standard Environment

* Notes from: https://cloud.google.com/appengine/docs/standard/java/quickstart

## Steps

### Use the GCP Console to create a new App Engine application: QuickStart01
* Configure the gcloud command-line environment, from windows CMD
```bash
gcloud init
gcloud auth application-default login
```

### Install the App Engine component:
```bash
gcloud components install app-engine-java
```

### Update to the latest version of Cloud SDK and all components:
```bash
gcloud components update 
```

### Install Maven, check: C:\jcabelloc\Notes\DevNotes\maven\maven.md, (Section: To Install Maven as independent Tool,)


### SHow installed components
```bash
gcloud components list
```

### Clone the Hello World sample app repository to your local machine:
```bash
git clone https://github.com/GoogleCloudPlatform/getting-started-java.git
```

### Change to the directory that contains the sample code:
```bash
cd getting-started-java/appengine-standard-java8/helloworld
```

### Start the local Jetty web server using the Jetty Maven plugin:
```bash
mvn appengine:run
```

## Deploying and running Hello World on App Engine
### Deploy the Hello World app
```bash
mvn appengine:deploy
```

### Launch your browser and view the app at http://YOUR_PROJECT_ID.appspot.com,
```bash
gcloud app browse
```