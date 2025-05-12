# conda_setup-until-2505
## 1. Data extracted from conda base environment from 2022 - 2025.05

```conda list``` to generate the list of all the installed packages

```conda list --export > packages2022-2025.txt``` With this command was exported 1 file packages2022-2025.txt

## 2. Data extracted using pip from 2022 - 2025.05

```conda env export > env-history_all.yml```to generate the list of all the installed packages and libraries 

```conda env export --from-history > env-history2022-2025_pip.yml```to generate the list of all the installed packages and libraries using pip

With this command were exported 2 files

## 3. Export file to generate a new environment in conda keeping all the packages and libraries used so far.

```conda env export > conda_base_environment2022-2025.yml```

## 4. Export file to generate a list with all the packages and libraries installed outside conda (using Powershell or other terminal).
```pip freeze > pip_global_requirements.txt```

## 5. Generate a new conda environment using a *.yml file.
```conda env create -f file_name.yml```
## 6. Activate the new environment.
```conda activate new_environment_name```
## 7. Update the new conda environment using a *.yml file.
```conda env update -f second_file.yml```

## 8. List all conda environments.
```conda env list```
