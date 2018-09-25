# Quickstart for Cloud SQL for MySQL
* Following steps from: https://cloud.google.com/sql/docs/mysql/quickstart

* Select or create a GCP project (quickstart01)
* Make sure that billing is enabled for your project


### Create a Cloud SQL instance
* Choose: MySQL Second Generation
* Instance ID: ayni
* root password: .....
* Mysql 5.7

### Connect to your instance using the mysql client in Cloud Shell

* In the Google Cloud Platform Console, click the Cloud Shell icon
* At the Cloud Shell prompt, connect to your Cloud SQL instance:
```bash
gcloud sql connect ayni --user=root
```


## Connecting to Cloud SQL from External Applications
* Based on: https://cloud.google.com/sql/docs/mysql/connect-external-app

### Enable the Cloud SQL Admin API.

### Install the proxy client on your local machine

### Determine how you will authenticate the proxy

* Options for authenticating the Cloud SQL Proxy
* Chosen: Credentials from an authenticated Cloud SDK client. 
* Check the current auth list
```bash        
gcloud auth list
```

### If required by your authentication method, create a service account
* nothing done
### Determine how you will specify your instances for the proxy
* nothing done

### Start the proxy
* Copy your instance connection name from the Instance details page.: quickstart01-216321:southamerica-east1:ayni
* Start the proxy. (From Windows CMD)
```bash
cloud_sql_proxy -instances=quickstart01-216321:southamerica-east1:ayni=tcp:3307
```

* NOTE: Using a service account and explicit instance specification (recommended for production environments):


### Connect from MySQL Workbench (for instance)
* Set up the parameters (localhost: 3307)

### Triggers cannot be created by default
* Error Message: Error Code: 1419. You do not have the SUPER privilege and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)
* Solution: Go to Google Cloud Platform and ADD a database flag: log_bin_trust_function_creators=on


## Granting access to App Engine
* IMPORTANT: It is valid if the app engine and the SQL DB are in different projects. In our case not. 
* Based on: https://cloud.google.com/appengine/docs/standard/java/cloud-sql/using-cloud-sql-mysql

### Identify the service account associated with your App Engine application
* demo01-216401@appspot.gserviceaccount.com

### 
* Go to the IAM & Admin Projects page in the Google Cloud Platform Console.
* Select the project that contains the Cloud SQL instance: quickstart01
* Search for the service account name: demo01-216401@appspot.gserviceaccount.com
* If the service account is already there, and it has a role that includes the cloudsql.instances.connect permission, you can proceed to Setting up.
* The Cloud SQL Client, Cloud SQL Editor and Cloud SQL Admin roles all provide the necessary permission, as do the legacy Editor and Owner project roles.
* Otherwise, add the service account by clicking Add.