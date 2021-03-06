# Google Cloud Platform Notes


## Set Up Environment


### Install Google Cloud SDK 
* Follow these steps at https://cloud.google.com/sdk/docs/quickstart-windows
* Selected path: C:\Users\jcabelloc\AppData\Local\Google\Cloud SDK
* After installing, login is required
* Then, it is required to select one of the projects, after selecting one, we have
```
* Commands that require authentication will use jcabelloc@gmail.com
* Commands will reference project `ayni-desa` by default
Run `gcloud help config` to learn how to change individual settings
```

* Then in the command line (CMD), we can ask for help: 
```
gcloud --help
```
* Some other handy commands
```
gcloud auth list
gcloud config list
gcloud info
gcloud help


gcloud projects list
gcloud config get-value project
gcloud config set account jcabelloc@itana.pe
gcloud config set project scr-qa
```

* Ordenando las configuraciones
```
gcloud config configurations list
gcloud config configurations activate default
gcloud config configurations delete abc-123
gcloud config configurations create jcabelloc
gcloud config set account jcabelloc@gmail.com
gcloud config set project cloudrun16-demo
```

### Set up Eclipse Environment

* Install Google Cloud Tools for Eclipse
* From inside Eclipse, select Help > Eclipse Marketplace... and search for Google Cloud.
* Restart Eclipse when prompted.



https://cloud.google.com/eclipse/docs/quickstart

### Run gcloud on Git Bash (add .cmd)
```bash
gcloud.cmd --help

```