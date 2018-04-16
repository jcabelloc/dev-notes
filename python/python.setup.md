# Python Basic Notes

### Install Python and Jupyter 
*  Install Python and Jupyter using the Anaconda Distribution: https://www.anaconda.com/download/#windows

* Open the Anaconda Prompt (Terminal)
* Change Directory (cd) to your desired location
* Run the Jupyter Notebook, in that terminal: 
```bash
jupyter notebook
```

### Customizing VS Code for Pyhton
* Install the Python extension
* Configure Settings for launching Anaconda Prompt

### Virtual Environments with Conda
* Create a virtual env
```bash
conda create --name myenv
```
* Create a virtual env with a package
```bash
conda create -n myenv scipy
```

* Create a virtual env with a particular python version and a package
```
conda create -n myenv python=3.4
```

* Activate the environment
```bash
activate firstEnv
```

* Deactivate the environment
```bash
deactivate
```

* List all your environments
```bash
conda info --envs
```


### Install Packages
* Install NumPy
```bash
conda install numpy
```

* Install Pandas
```bash
conda install pandas
```