

```conda info --envs```

```conda env list```

```conda deactivate```
```conda env remove --name superset_env39``
```conda create -n superset_new python=3.9```
```pip show apache-superset```

* Clear Cache (Optional but Recommended) Superset might be caching some settings. Clear the cache by running the following command: 
```superset reset```

* List all installed packages in your environment:
```conda list```
```conda list -n default_env```

* expost the list of packages in the current python environmeent to the current directory

```conda list --export > <file_name.txt>```

*  delete the ones you want to remove using
```conda remove <package_name>```
```conda remove package1 package2 package3```

### CMD
* To open a folder in vs code
```code <path>```
* Open the current directory in vs code
```code .```


### environments_python on windows 10
* create the environment "my_project" with python=3.10
```mkdir my_project && cd my_project```<br>
```py -m venv <environment_name> python=3.10```<br>

* activate the environment
```<environment_name>\Scripts\activate```

* deactivate the environment
```deactivate```

* install the packages in the environment
```pip install <package_name>```

* list of packages installed
```pip list```

* list of environments installed : Python doesn't provide a direct command to list all virtual environments, so managing them requires keeping track of where you created them or using tools that can help organize them for you.

* Delete the Virtual Environment Folder : Open File Explorer and navigate to the location where your environment is stored.Right-click the environment folder (e.g., <environment_name>) and select Delete.<br>
Alternatively, you can delete the virtual environment via the command line:

```rmdir /s /q <environment_path>```

The ```/s``` flag removes all files and subdirectories, and ```/q``` ensures it deletes without asking for











