# GCP Cloud Shell Notes
* Reference: https://cloud.google.com/shell/docs/examples


You can use Google Cloud Shell to:

* Create and manage Google Compute Engine instances
* Create and access Google Cloud SQL databases
* Manage Google Cloud Storage data
* Interact with hosted or remote Git repositories, including Google Cloud Source Repositories
* Build and deploy Google App Engine applications

### Handy commands
* Delete a configuration
```
gcloud config configurations delete test-config
```

* To set your Cloud Platform project in this session use “gcloud config set project [PROJECT_ID]”
```
gcloud config set project kubernetes01-239613
```

```
gcloud compute images list
gcloud compute instances list
```