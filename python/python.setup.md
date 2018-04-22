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
conda create --name firstEnv
```
* Create a virtual env with a package
```bash
conda create -n firstEnv scipy
```

* Create a virtual env with a particular python version and a package
```
conda create -n firstEnv python=3.4
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

### Libraries for Data Input and Output
```bash
conda install sqlalchemy
conda install lxml
conda install html5lib
conda install BeautifulSoup4
```


### Matplotlib
```bash
conda install matplotlib
```

### Seaborn
```bash
conda install seaborn
```

### Plotly and cufflinks
```bash
conda install plotly
```

Cufflinks is not currently available through conda but it is available through pip
```bash
pip install cufflinks
```

