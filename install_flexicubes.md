### Instalación de FlexiCubes en Windows (actualizado)

Este instructivo documenta todos los pasos requeridos para compilar y ejecutar correctamente FlexiCubes en Windows con PyTorch 1.12.0, CUDA 11.3 y Kaolin 0.15.0.

---

#### 1. Instalar Visual Studio 2019 Community

Descargar desde: [https://visualstudio.microsoft.com/vs/older-downloads/](https://visualstudio.microsoft.com/vs/older-downloads/)

Componentes requeridos:

- **MSVC v14.29 (v142)**
- **Windows 10 SDK (10.0.19041.0)**
- **C++ Desktop Development Tools**

Verifica que el archivo `basetsd.h` exista en la siguiente carpeta (o dentro de C:\Program Files (x86)\Windows Kits\10\Include\10.0.19041.0\):

```
C:\Program Files (x86)\Windows Kits\10\Include\10.0.19041.0\shared\basetsd.h
```

Si no, instala manualmente el SDK desde:
[https://developer.microsoft.com/en-us/windows/downloads/sdk-archive/](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive/)

---

#### 2. Crear entorno Conda y activar

```bash
conda create -n flexicubesconda python=3.9 -y
conda activate flexicubesconda
```

---

## 2.1: Instalar `cudatoolkit` 11.3 dentro del entorno

```bash
conda install -c conda-forge cudatoolkit=11.3
```

---

#### 3. Instalar dependencias principales

```bash
pip install torch==1.12.0+cu113 torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu113
-------en caso de error probar pip install torch==1.12.0+cu113 torchvision==0.13.0+cu113 torchaudio==0.12.0 --extra-index-url https://download.pytorch.org/whl/cu113 -------
pip install kaolin==0.15.0 -f https://nvidia-kaolin.s3.us-east-2.amazonaws.com/torch-1.12.0_cu113.html
pip install imageio trimesh tqdm matplotlib ninja
```

---

#### 4. Clonar repositorios necesarios

```bash
git clone https://github.com/nv-tlabs/FlexiCubes.git
cd FlexiCubes
git clone https://github.com/NVlabs/nvdiffrast.git
```

---

#### 5. Crear archivo `setup_flexicubes_vs2019.bat` dentro de `nvdiffrast`

```bat
@echo off
CALL "%USERPROFILE%\anaconda3\Scripts\activate.bat" flexicubesconda

:: Configurar entorno para compilación con MSVC
SET DISTUTILS_USE_SDK=1
SET MSSdk=1
SET CL=/D_HAS_ITERATOR_DEBUGGING=0 /MD /EHsc
SET LINKFLAGS=/DEFAULTLIB:libcpmt.lib

:: Incluir headers del SDK de Windows
SET INCLUDE=%INCLUDE%;C:\Program Files (x86)\Windows Kits\10\Include\10.0.19041.0\shared

:: Cambiar a esta carpeta
CD /D %~dp0

:: Asegurar versión de numpy compatible
pip install numpy==1.23.5

:: Compilar e instalar nvdiffrast
python -m pip install . --force-reinstall --no-cache-dir

pause
```

---

#### 6. Abrir terminal de compilación correcta

Abre el siguiente acceso directo de Visual Studio 2019:

```
► x64 Native Tools Command Prompt for VS 2019
```

---

#### 7. Ejecutar script de instalación

```bash
cd ruta\a\FlexiCubes\nvdiffrast
setup_flexicubes_vs2019.bat
```

---

#### 8. Ejecutar ejemplo de optimización

```bash
cd ..\examples
python optimize.py --ref_mesh data/inputmodels/block.obj --out_dir out/block
```

---

#### Resultado esperado

```
optimization step 0/100, loss: ...
```

✔  FlexiCubes funcionando correctamente.
