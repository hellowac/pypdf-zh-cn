# 安装指南  

安装 pypdf 有多种方式，其中最常见的是使用 pip。  

## 使用 pip 安装  

pypdf 需要 Python 3.8 或更高版本才能运行。  

通常，Python 自带 `pip`（一个包管理工具）。通过 pip，可以安装 pypdf：  

```bash  
pip install pypdf  
```  

如果您不是超级用户（系统管理员 / root），也可以仅为当前用户安装 pypdf：  

```bash  
pip install --user pypdf  
```  

### 可选依赖  

pypdf 尽可能做到自包含，但对于某些任务（如加密和图像格式处理），维护代码的工作量过大。  

如果希望安装所有可选依赖项，请运行：  

```bash  
pip install pypdf[full]  
```  

或者，您也可以仅安装部分依赖：  

如果您计划使用 pypdf 对使用 AES 加密的 PDF 进行加解密操作，需要安装额外依赖。RC4 加密支持普通安装即可使用：  

```bash  
pip install pypdf[crypto]  
```  

如果您计划使用图像提取功能，需要安装 Pillow：  

```bash  
pip install pypdf[image]  
```  

## Python 版本支持  

从 pypdf 4.0 开始，每个版本（包括小版本）都支持所有受支持的 [Python 版本](https://devguide.python.org/versions/)，并与所有现有的 Python 版本兼容（不包括已停止支持的版本）。  

早期版本的 pypdf 支持以下 Python 版本：  

| Python                 | 3.11 | 3.10 | 3.9 | 3.8 | 3.7 | 3.6 | 2.7 |  
| ---------------------- |:----:|:----:|:---:|:---:|:---:|:---:|:---:|  
| pypdf 3.x              | ✅   | ✅   | ✅  | ✅  | ✅  | ✅  | ❌  |  
| PyPDF2 >= 2.0          | ✅   | ✅   | ✅  | ✅  | ✅  | ✅  | ❌  |  
| PyPDF2 1.20.0 - 1.28.4 | ❌   | ✅   | ✅  | ✅  | ✅  | ✅  | ✅  |  
| PyPDF2 1.15.0 - 1.20.0 | ❌   | ❌   | ❌  | ❌  | ❌  | ❌  | ✅  |  

## Anaconda  

Anaconda 用户可以通过 [conda-forge 安装 pypdf](https://anaconda.org/conda-forge/pypdf)。  

## 开发版本  

如果您想使用正在开发的当前版本，请运行以下命令：  

```bash  
pip install git+https://github.com/py-pdf/pypdf.git  
```  