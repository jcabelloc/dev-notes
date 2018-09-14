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