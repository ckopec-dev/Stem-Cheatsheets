



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

