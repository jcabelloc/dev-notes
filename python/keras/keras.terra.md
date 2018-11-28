# Notes about running Keras with Tensorflow in Linux (Terra Cluster)

### Set up environment 

```bash
[jcabelloc@terra2 project]$ module load Python/3.5.2-intel-2017A
[jcabelloc@terra2 project]$ module load cuDNN/7.0.5-CUDA-9.0.176
[jcabelloc@terra2 project]$ virtualenv venv
[jcabelloc@terra2 project]$ source venv/bin/activate
(venv) [jcabelloc@terra2 project]$ pip3 install numpy  # not required, tensorflow use and install a specific version 
(venv) [jcabelloc@terra2 project]$ pip3 install tensorflow==1.10.0
(venv) [jcabelloc@terra2 project]$ pip3 install tensorflow-gpu==1.10.0
(venv) [jcabelloc@terra2 project]$ pip3 install keras
(venv) [jcabelloc@terra2 project]$ pip3 install pillow

```

### pip useful commands
```bash
# Uninstall a package
$ pip uninstall numpy

# List packages
$ pip list

```

### Managing enviornments
```bash
# Deactivate an environment
(venv) [jcabelloc@terra2 project]$  deactivate
# Delete a virtual environment
$ rm -r venv

```

### Download datasets from Kaggle
To use the Kaggle API, sign up for a Kaggle account at https://www.kaggle.com. Then go to the 'Account' tab of your user profile (https://www.kaggle.com/<username>/account) and select 'Create API Token'. This will trigger the download of kaggle.json, a file containing your API credentials. Place this file in the location ~/.kaggle/kaggle.json
```bash
(venv) [jcabelloc@terra1 project]$ pip3 install kaggle
(venv) [jcabelloc@terra1 project]$ kaggle datasets download -d rajmehra03/cats-and-dogs-images
```