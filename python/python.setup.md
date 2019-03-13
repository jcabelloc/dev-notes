# Python Basic Notes

### Install Python and Jupyter 
*  Install Anaconda Distribution: https://www.anaconda.com/distribution/. It includes python and jupyter notebook by default
* Selected installation path: C:\jcabelloc\Programs\Anaconda3

* Check the getting started guide: http://docs.anaconda.com/anaconda/user-guide/getting-started/

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

### Conda common commands
* Check out conda version
```
> conda -V
```

* Update conda and then all packages
```
> conda update -n root conda
> conda update --all
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

### List Linked packages
```bash
conda list
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


### Package for Machine Learning
```bash
conda install scikit-learn
```

### Natural Language Text Processing Kit
```bash
conda install nltk
```