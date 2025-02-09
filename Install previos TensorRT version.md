This tutorail to install tensorrt-8.5 in ArchLinux
Tensorrt-8.5.3.1 | gcc11 | cuda 11.7 | cudnn8.5.9.96
---------------------------------------------------
#1: Install gcc11
export MAKEFLAGS="-j$(nproc)" (speed up the compile of gcc11 pkg)
sudo paru -S gcc11
echo export PATH=/usr/lib/gcc/x86_64-pc-linux-gnu/11.4.0:$PATH >> /.bashrc
## check installation success:
gcc11 --version

#2: Install cuda
sudo paru -S cuda-11.7
echo export PATH=/opt/cuda-11.7/bin:$PATH >> /.bashrc
echo export LD_LIBRARY_PATH=/opt/cuda-11.7/lib64:$LD_LIBRARY_PATH >> /.bashrc
## check installation success:
nvcc --version

#3: Install cudnn
wget https://developer.download.nvidia.com/compute/redist/cudnn/v8.5.0/local_installers/11.7/cudnn-linux-x86_64-8.5.0.96_cuda11-archive.tar.xz
tar -xvf cudnn-linux-x86_64-8.5.0.96_cuda11-archive.tar.xz
sudo cp cudnn-*-archive/include/cudnn*.h /opt/cuda-11.7/include 
sudo cp -P cudnn-*-archive/lib/libcudnn* /opt/cuda-11.7/lib64 
sudo chmod a+r /opt/cuda-11.7/include/cudnn*.h /opt/cuda-11.7/lib64/libcudnn*

#4: Install tensorRT 8.5.3.1
Dowload previous tensorRt verion in: https://developer.nvidia.com/tensorrt/download
tar -xvzf cudnn-linux-x86_64-8.5.0.96_cuda11-archive.tar.xz /opt/tensorrt8.5
echo export PATH=/opt/tensorrt8.5bin:$PATH >> /.bashrc
echo export LD_LIBRARY_PATH=/opt/tensorrt8.5/lib:$LD_LIBRARY_PATH >> /.bashrc
 ## Binding to Conda env
conda activate myenv
pip install /opt/TensorRT-<version>/python/tensorrt-<version>.whl
## Run test script
print("Hello, world!")

import tensorrt as trt
print("tensort version: ", trt.__version__)

import torch
print("torch is available:" , torch.cuda.is_available(), torch.__version__)

import tensorrt as trt
TRT_LOGGER = trt.Logger(trt.Logger.WARNING)
builder = trt.Builder(TRT_LOGGER)
print("TensorRT Builder created successfully!")
## Warning
If facing with libnvinfer_builder_resource.so.8.6.1: cannot enable executable stack as shared object requires: Invalid argument
 - Use the execstack tool to check if the library requires an executable stack:

execstack -q /path/to/libnvinfer_builder_resource.so.8.6.1
Output Interpretation:
X: The library requires an executable stack.
-: The library does not require an executable stack.

sudo execstack -c /path/to/libnvinfer_builder_resource.so.8.6.1

execstack -q /path/to/libnvinfer_builder_resource.so.8.6.1
You should now see a - instead of X.
