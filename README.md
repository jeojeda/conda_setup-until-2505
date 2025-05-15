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

# New environment and install Flexicubes
# Instalación correcta de FlexiCubes en Windows

Este instructivo documenta todos los pasos requeridos y probados para compilar y ejecutar correctamente [FlexiCubes](https://github.com/nv-tlabs/FlexiCubes) en Windows con CUDA.

---

## 1. Crear entorno Conda
```bash
conda create -n flexicubesconda python=3.9 -y
conda activate flexicubesconda

pip install torch==1.12.0+cu113 torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu113

pip install kaolin==0.15.0 -f https://nvidia-kaolin.s3.us-east-2.amazonaws.com/torch-1.12.0_cu113.html

git clone https://github.com/nv-tlabs/FlexiCubes.git
cd FlexiCubes



# 🧱 Instalación de FlexiCubes con PyTorch 1.12.0, CUDA 11.3 y Kaolin 0.15.0 (first try)

Este entorno ha sido probado con éxito en Windows 10/11. Sigue los pasos a continuación para reproducir la instalación y ejecución del repositorio [FlexiCubes](https://github.com/nv-tlabs/FlexiCubes).

---

## 🔧 Requisitos previos

- Anaconda
- CUDA Toolkit **11.3** (descargar desde [NVIDIA](https://developer.nvidia.com/cuda-11.3.0-download-archive))
- Visual Studio 2019 Community Edition con:
  - **MSVC v14.29**
  - **Windows 10 SDK (10.0.19041.0)**
  - **C++ CMake tools for Windows**

---

## 🐍 Paso 1: Crear entorno Conda

```bash
conda create -n flexicubesconda python=3.9 -y
conda activate flexicubesconda
```

---

## 📦 Paso 2: Instalar PyTorch compatible con CUDA 11.3

```bash
pip install torch==1.12.0 torchvision==0.13.0 --extra-index-url https://download.pytorch.org/whl/cu113
```

---

## 📦 Paso 3: Instalar Kaolin 0.15.0 para PyTorch 1.12.0 + cu113

```bash
pip install kaolin==0.15.0 -f https://nvidia-kaolin.s3.us-east-2.amazonaws.com/torch-1.12.0_cu113.html
```

---

## 📦 Paso 4: Instalar dependencias adicionales

```bash
pip install imageio trimesh tqdm matplotlib ninja
```

---

## ⚠️ Paso 5: Configurar compilador

- Ejecutar desde el **Developer Command Prompt for VS2019 (x64)**.
- Verificar que el compilador `cl.exe` esté disponible:

```bash
cl /Bv
```

- Confirmar que las variables de entorno incluyen los headers de Windows Kits:

```bash
echo %INCLUDE%
```

Debe incluir rutas similares a:

```
C:\Program Files (x86)\Windows Kits\10\Include\10.0.19041.0\shared
C:\Program Files (x86)\Windows Kits\10\Include\10.0.19041.0\um
```

- Si falta `stddef.h`, **reparar Visual Studio 2019** y asegurarse de haber instalado el SDK correcto.

---

## 📂 Paso 6: Clonar y entrar al repositorio FlexiCubes

```bash
git clone https://github.com/nv-tlabs/FlexiCubes.git
cd FlexiCubes
```

---

## ▶️ Paso 7: Ejecutar optimizador

```bash
python examples/optimize.py --ref_mesh data/inputmodels/block.obj --out_dir out/block
```

---

## ✅ Notas finales

- Este entorno **no utiliza CUDA 11.8**, por lo que puede desinstalarse si no se usa en otros proyectos.
- Todos los pasos anteriores fueron validados manualmente en un entorno real hasta lograr una ejecución exitosa.


# Instalación completa de FlexiCubes en Windows

# 1. Crear entorno Conda
conda create -n flexicubesconda python=3.9 -y
conda activate flexicubesconda

# 2. Instalar PyTorch con CUDA 11.3
pip install torch==1.12.0+cu113 torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu113

# 3. Instalar Kaolin compatible
pip install kaolin==0.15.0 -f https://nvidia-kaolin.s3.us-east-2.amazonaws.com/torch-1.12.0_cu113.html

# 4. Clonar FlexiCubes
git clone https://github.com/nv-tlabs/FlexiCubes.git
cd FlexiCubes

# 5. Clonar nvdiffrast si no está
git clone https://github.com/NVlabs/nvdiffrast.git
cd nvdiffrast

# 6. Instalar Visual Studio 2019 Community con:
# - MSVC v14.29 (v142)
# - Windows 10 SDK (10.0.19041.0)
# - C++ Desktop Development

# Verifica que el archivo exista:
# C:\Program Files (x86)\Windows Kits\10\Include\10.0.19041.0\shared\basetsd.h
# Si no, instala manualmente el SDK desde:
# https://developer.microsoft.com/en-us/windows/downloads/sdk-archive/

# 7. Crear archivo setup_flexicubes_vs2019.bat en nvdiffrast con:

@echo off
CALL "%USERPROFILE%\anaconda3\Scripts\activate.bat" flexicubesconda
SET DISTUTILS_USE_SDK=1
SET MSSdk=1
SET CL=/D_HAS_ITERATOR_DEBUGGING=0 /MD /EHsc
SET LINKFLAGS=/DEFAULTLIB:libcpmt.lib
SET INCLUDE=%INCLUDE%;C:\Program Files (x86)\Windows Kits\10\Include\10.0.19041.0\shared
CD /D %~dp0
python -m pip install . --force-reinstall --no-cache-dir
pause

# 8. Ejecutar el script
cd nvdiffrast
..\setup_flexicubes_vs2019.bat

# 9. Si hay error binario con numpy:
pip install numpy==1.23.5

# 10. Ejecutar ejemplo
cd examples
python optimize.py --ref_mesh data/inputmodels/block.obj --out_dir out/block

# Resultado esperado
# optimization step 0/100, loss: ...
# => ¡FlexiCubes funcionando correctamente!

