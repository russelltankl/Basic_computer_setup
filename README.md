# Basic_computer_setup
This repository is a reference for new computers which needs basic setup.


# Windows
## Python
### Installation and setup
I usually use dual python version on my computer. Click [here](https://www.python.org/downloads/windows/) to find the Python version you want to install. (Choose 64bit python versions)

During installation of both Python versions, make sure to not add them into PATH and install the location to C:\ .

Afterwords, go to System Environment Variables > Environment Variables > User Variables _ Path. Add the following lines into PATH.

```
C:\Python27
C:\Python27\Scripts
C:\Python37
C:\Python37\Scripts
```

After that, change .exe of each Python version to eg: python.exe > python2.exe.

If using Git Bash, follow the steps below to ensure proper invocation of python. Restart Git Bash after following these steps.

```
cd ~
touch .bashrc
alias py2='winpty python2.exe'
alias py3='winpty python3.exe'
```

### Usage

To start Python interpreter

#### Python2
```
py2
```

#### Python3
```
py3
```

To install packages

#### Python2
```
pip install numpy
```
or
```
py2 -m pip install numpy
```
#### Python3
```
pip3 install numpy
```
or
```
pip3 -m install numpy
```

## Git


## Matlab
To setup Matlab Engine API, first you have to install Matlab first.

# Linux (Ubuntu)
After some trial and errors, installation works best by performing these in order.
## Tensorflow GPU w/ CUDA 9.2

### Update and upgrade system
```
sudo apt-get update
sudp apt-get upgrade
```

### Verify CUDA-capable GPU
```
lspci | grep -i nvidia
```

### Verify supported Linux version
```
uname -m && cat /etc/*release
```

### Install dependencies
```
sudo apt-get install build-essential 
sudo apt-get install cmake git unzip zip
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python2.7-dev python3.5-dev python3.6-dev pylint
```

### Install Linux kernel header
```
sudo apt-get install linux-headers-$(uname -r)
```

### Install NVIDIA CUDA 9.2
```
sudo apt-get purge nvidia*
sudo apt-get autoremove
sudo apt-get autoclean
sudo rm -rf /usr/local/cuda*

sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
echo "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64 /" | sudo tee /etc/apt/sources.list.d/cuda.list
```

### Reboot and export environment variables
```
echo 'export PATH=/usr/local/cuda-9.2/bin${PATH:+:${PATH}}' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-9.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}' >> ~/.bashrc
source ~/.bashrc
sudo ldconfig
nvidia-smi
```

### Install cuDNN 7.1.4
Install this version because the latest tensorRT supports this version currently and now the newest cuDNN.

```
cuDNN v7.1.4 Library for Linux
```

```
tar -xf cudnn-9.2-linux-x64-v7.1.tgz
sudo cp -R cuda/include/* /usr/local/cuda-9.2/include
sudo cp -R cuda/lib64/* /usr/local/cuda-9.2/lib64
```

### Install NCCL 2.2.13
```
Download NCCL v2.2.13, for CUDA 9.2 -> NCCL 2.2.13 O/S agnostic and CUDA 9.2
```

```
tar -xf nccl_2.2.13-1+cuda9.2_x86_64.txz
cd nccl_2.2.13-1+cuda9.2_x86_64
sudo cp -R * /usr/local/cuda-9.2/targets/x86_64-linux/
sudo ldconfig
```

### Install dependencies
```
sudo apt-get install libcupti-dev
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
```

For Python2.7
```
sudo apt-get install python-numpy python-dev python-pip python-wheel
```

For Python3.n
```
sudo apt-get install python3-numpy python3-dev python3-pip python3-wheel
```

### Configure Tensorflow from source
```
cd ~/
wget https://github.com/bazelbuild/bazel/releases/download/0.14.0/bazel-0.14.0-installer-linux-x86_64.sh
chmod +x bazel-0.14.0-installer-linux-x86_64.sh
./bazel-0.14.0-installer-linux-x86_64.sh --user
echo 'export PATH="$PATH:$HOME/bin"' >> ~/.bashrc

source ~/.bashrc
sudo ldconfig

cd ~/
git clone https://github.com/tensorflow/tensorflow.git
cd tensorflow
git pull
git checkout r1.8
```

#### Python versions
If you want to build in 2.7 or 3.5, safest bet is to activate a virtualenv with python specified. After you activated virtualenv, then navigate into tensorflow folder to configure tensorflow. (Could also do this without virtualenv I think)
```
./configure
```

The configuration process as follows:
```
Please specify the location of python. [Default is /usr/bin/python]: /usr/bin/python3
Do you wish to build TensorFlow with jemalloc as malloc support? [Y/n]: Y
Do you wish to build TensorFlow with Google Cloud Platform support? [Y/n]: Y
Do you wish to build TensorFlow with Hadoop File System support? [Y/n]: Y
Do you wish to build TensorFlow with Amazon S3 File System support? [Y/n]: Y
Do you wish to build TensorFlow with Apache Kafka Platform support? [Y/n]: Y
Do you wish to build TensorFlow with XLA JIT support? [y/N]: N
Do you wish to build TensorFlow with GDR support? [y/N]: N
Do you wish to build TensorFlow with VERBS support? [y/N]: N
Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]: N
Do you wish to build TensorFlow with CUDA support? [y/N]: Y
Please specify the CUDA SDK version you want to use, e.g. 7.0. [Leave empty to default to CUDA 9.0]: 9.2
Please specify the location where CUDA 9.2 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: /usr/local/cuda-9.2
Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 7.0]: 7.1.4
Please specify the location where cuDNN 7 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda-9.2]: /usr/local/cuda-9.2
Do you wish to build TensorFlow with TensorRT support? [y/N]: N
Please specify the NCCL version you want to use. [Leave empty to default to NCCL 1.3]: 2.2
Please specify the location where NCCL 2 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda-9.2]: /usr/local/cuda-9.2/targets/x86_64-linux
Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 5.0] 5.0
Do you want to use clang as CUDA compiler? [y/N]: N
Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/gcc]: /usr/bin/gcc
Do you wish to build TensorFlow with MPI support? [y/N]: N
Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]: -march=native
Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]:N
```

If you want to configure Tensorflow witn TensorRT, you'll need to install that first.

### Build Tensorflow using bazel
This will take awhile so go get some sleep after typing this. NOTE: Make sure to upgrade pip to the latest version, if not this will fail at the last step.

#### CPU
```
bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package
```

#### GPU
```
bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package
```

After the long installation and hopefully it was successful. Run the build_pip_pakage.sh shell file to generate .whl based on what python version you defined at the start of the configuration.
```
bazel-bin/tensorflow/tools/pip_package/build_pip_package tensorflow_pkg
cd tensorflow_pkg
sudo apt-get install virtualenv
virtualenv tf_1.8.0_cuda9.2 -p /usr/bin/python3
source tf_1.8.0_cuda9.2/bin/activate
pip install tensorflow*.whl
```

### Misc
Bugs I encountered while installing Tensorflow.

#### Python2.7
- Import Error: no module named enum

```
pip install enum34
```

- Import Error: no module named mock

```
pip install mock
```

- tensorflow/core/kernels/tile_functor_gpu.cu.pic.o' was not created [Source](https://github.com/tensorflow/tensorflow/issues/3786#issuecomment-239646852)

```
edit tensorflow/third_party/gpus/crosstool/CROSSTOL and add:
cxx_builtin_include_directory: "path/to/cuda/include" (note: symlink did not work, use actual path)
```

### Sources
- [Building Tensorflow](https://wagonhelm.github.io/articles/2018-02/Installing-GPU-Supported-Tensorflow)
