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

# New environment and install Kaolin
## Ref. from https://github.com/nv-tlabs/FlexiCubes?tab=readme-ov-file
1. ```conda create --name FlexiCubes```

2. ```conda activate FlexiCubes```

3. ```pip install kaolin==0.17.0 -f https://nvidia-kaolin.s3.us-east-2.amazonaws.com/torch-2.0.1_cu118.html```

4. ```conda install pytorch==1.12.0 torchvision==0.13.0 torchaudio==0.12.0 cudatoolkit=11.3 -c pytorch```

5. ```pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118```

6. ```pip install imageio trimesh tqdm matplotlib torch_scatter ninja```

//conda env remove --name flexicubes  //
## Install CUDA v 11.8 https://developer.nvidia.com/cuda-11-8-0-download-archive?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local
# Anaconda Prompt (run as administrator)
1.```conda update conda -y```
2.```conda create -n flexicubestorch python=3.9 -y```
3.```conda activate flexicubestorch```
4.```pip install torch==2.2.2 torchvision==0.17.2 torchaudio==2.2.2 --index-url https://download.pytorch.org/whl/cu118```
#change to C:\Users\user_name>
5.```pip install flask ipython "jupyter-client<8" tornado tqdm lxml XlsxWriter```
6.```conda env export --name flexicubestorch > flexicubestorch_env.yml```




