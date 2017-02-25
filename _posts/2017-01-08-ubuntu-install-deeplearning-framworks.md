---
layout: post
title: Ubuntu14.04+深度学习框架
categories: [Ubuntu, Deep Learning]
tags: Deep Learning
---

首先需要一个科学上网的环境，很多东西的安装，遇到问题时查找解决方法，都需要跨过那道该死的墙，知道如何科学跨墙是必须的。

一、安装Ubuntu14.04(Linux的一个发行版）

Ubuntu14.04的安装比较简单，网上Google教程即可，需要注意的是分区，分区设置如下：

/boot >=200M 逻辑分区 Ext4文件系统  
/ 越大越好 逻辑分区 Ext4文件系统  
/home 越大越好 逻辑分区 Ext4文件系统  
/tmp 10000M 逻辑分区 Ext4文件系统  
/swap =内存大小 交换空间  

二、安装cuda 8.0（GPU Toolkit）

先安装一些常用的软件：

    sudo apt-get install g++
    sudo apt-get install git
    sudo apt-get install freeglut3-dev

在cuda官网上下载好cuda_8.0.44_linux.run，cudnn-8.0-linux-x64-v5.1.tgz，放到根目录下

1、禁用nouveau驱动

按Ctrl+Alt+F1进入命令提示符,新建一个黑名单文件:

    sudo vim /etc/modprobe.d/blacklist-nouveau.conf

输入：

    blacklist nouveau
    options nouveau modset=0

保存退出（:wq）

然后执行:

    sudo update-initramfs -u

执行 lspci | grep nouveau查看是否有内容:

    lspci | grep nouveau

如果没有内容 ，说明禁用成功，如果有内容，就重启一下再查看:

    sudo reboot

重启后，进入登录界面的时候，不要登录进入桌面，直接按Ctrl+Alt+F1进入命令提示符

2、安装

停止桌面服务：

    sudo service lightdm stop

进入~根据目录安装cuda 8.0

    cd ~
    sudo sh cuda_8.0.44_linux.run

安装的时候，要让你先看一堆文字（EULA)，直接不停的按空格键到100%，然后输入一堆accept,yes,yes或回车进行安装

安装完成后，重启

执行nvidia-smi，如果能够显示显卡信息，说明安装成功

否则需要卸载重装，卸载命令如下：

    sudo /usr/local/cuda-8.0/bin/uninstall_cuda_8.0.pl
    sudo /usr/bin/nvidia-uninstall

如果还是不放心是否安装成功，可以编译samples进行测试

3、配置环境变量：

配置~/.bashrc：

    sudo vim ~/.bashrc

在最后面添加：

    export CUDA_HOME=/usr/local/cuda-8.0
    export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

配置/etc/bash.bashrc：

    sudo vim /etc/bash.bashrc

在最后面添加：

    export CUDA_HOME=/usr/local/cuda-8.0
    export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

保存退出，至此cuda 8.0安装完毕

测试是否安装成功：

    nvcc -V

会得到相应的nvcc编译器相应的信息，那么CUDA配置成功了，记得重启系统！

三、安装cudnn5.1（GPU加速）

    tar xvzf cudnn-8.0-linux-x64-v5.1.tgz
    sudo cp cuda/include/cudnn.h /usr/local/cuda-8.0/include
    sudo cp cuda/lib64/libcudnn* /usr/local/cuda-8.0/lib64
    sudo chmod a+r /usr/local/cuda-8.0/include/cudnn.h /usr/local/cuda-8.0/lib64/libcudnn*

四、安装Anaconda（集成的Python开发环境）

1、官网下载 Anaconda2-4.2.0-Linux-x86_64.sh

2、执行：

    ./Anaconda2-4.2.0-Linux-x86_64.sh

在安装的过程中，会问你安装路径，直接回车默认就可以

有个地方问你是否将anaconda安装路径加入到环境变量（.bashrc)中，这个一定要输入yes

安装成功后，会有当前用户根目录下生成一个anaconda2的文件夹，里面就是安装好的内容

输入conda list 就可以查询，你现在安装了哪些库，常用的numpy, scipy名列其中

如果你还有什么包没有安装上，可以运行 conda install ***  来进行安装

如果某个包版本不是最新的，运行 conda update *** 就可以了

安装完成后，可以在终端执行：

    jupyter notebook

或者

    spyder

进行代码编辑、运行

也可以安装PyCharm（十分强大的Python IDE）、Atom（十分强大的编辑器）等

注：

安装好Anaconda后就开始安装深度学习框架了，在正式安装深度学习框架之前，先提及一个Anaconda的强大功能：conda environment

Anaconda不仅集成了大量的python科学计算库、开发环境，还提供了一个“conda”管理工具，使用它能创建不同的虚拟环境，各个虚拟环境之间的依赖项互不干扰

基于此，创建一个专门用于深度学习开发的深度学习开发环境，在这个环境里面安装深度学习框架，不会影响原来的环境

在终端执行：

    conda create -n deeplearning --clone root

来新建一个虚拟环境，命名为deeplearning

执行：

    source activate deeplearning

来激活此环境

执行：

    source deactivate

来退出此环境

删除虚拟环境的命令为：

    conda remove -n deeplearning --all

五、安装Tensorflow（开源的符号主义的张量操作框架，由Google开发）

1、进入deeplearning虚拟环境

    source activate deeplearning

2、安装tensorflow

使用GPU加速：

    export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.11.0-cp27-none-linux_x86_64.whl
    pip install --ignore-installed --upgrade $TF_BINARY_URL

安装完成后，可以通过以下代码简单测试是否安装成功：

```
$python
...
>>> import tensorflow as tf
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> print(sess.run(hello))
Hello, TensorFlow!
>>> a = tf.constant(10)
>>> b = tf.constant(32)
>>> print(sess.run(a + b))
42
>>>
```

六、安装Theano（开源的符号主义张量操作框架，由蒙特利尔大学LISA/MILA实验室开发）

    pip install Theano

安装完成后，配置theano文件：

    gedit ~/.theanorc

如果使用GPU加速，写入以下：

```
[global]
openmp=False
device = gpu
floatX = float32
allow_input_downcast=True
[lib]
cnmem = 0.8
[blas]
ldflags= -lopenblas
[nvcc]
fastmath = True
```

如果不使用GPU加速，则写入：

```
[global]
openmp=True
device = cpu
floatX = float32
allow_input_downcast=True
[blas]
ldflags= -lopenblas
```

可以通过以下代码简单测试是否安装成功：

    python -c "import theano; theano.test()"

七、安装Keras（上层深度学习框架，后端为Tensorflow或Theano）

    pip install keras

如果想改变使用的后端：

    gedit ~/.keras/keras.json

在这里面配置

八、安装caffe（C++深度学习框架，有matlab、python接口）

1、先安装一堆第三方库

```
sudo apt-get install libatlas-base-dev
sudo apt-get install libprotobuf-dev
sudo apt-get install libleveldb-dev
sudo apt-get install libsnappy-dev
sudo apt-get install libopencv-dev
sudo apt-get install libboost-all-dev
sudo apt-get install libhdf5-serial-dev
sudo apt-get install libgflags-dev
sudo apt-get install libgoogle-glog-dev
sudo apt-get install liblmdb-dev
sudo apt-get install protobuf-compiler
```

2、安装opencv

```
cd caffe
sudo git clone https://github.com/jayrambhia/Install-OpenCV
cd Install-OpenCV/Ubuntu
sudo sh dependencies.sh
cd 2.4
sudo sh opencv2_4_10.sh
```

3、将caffe根目录下的python文件夹加入到环境变量

    sudo gedit ～/.bashrc

在最后面加入:

    export PYTHONPATH=/home/ray/caffe/python:$PYTHONPATH

保存退出，更新配置文件:

    sudo ldconfig

4、修改编译配置文件Makefile.config

    cd ～/caffe
    sudo cp Makefile.config.example Makefile.config
    sudo gedit Makefile.config

按照下面修改（使用GPU，使用cudnn加速，提供了python、matlab的接口）：

```
## Refer to http://caffe.berkeleyvision.org/installation.html
# Contributions simplifying and improving our build system are welcome!
# cuDNN acceleration switch (uncomment to build with cuDNN).
USE_CUDNN := 1
# CPU-only switch (uncomment to build without GPU support).
# CPU_ONLY := 1
# uncomment to disable IO dependencies and corresponding data layers
# USE_OPENCV := 0
# USE_LEVELDB := 0
# USE_LMDB := 0
# uncomment to allow MDB_NOLOCK when reading LMDB files (only if necessary)
#   You should not set this flag if you will be reading LMDBs with any
#   possibility of simultaneous read and write
# ALLOW_LMDB_NOLOCK := 1
# Uncomment if you're using OpenCV 3
# OPENCV_VERSION := 3
# To customize your choice of compiler, uncomment and set the following.
# N.B. the default for Linux is g++ and the default for OSX is clang++
# CUSTOM_CXX := g++
# CUDA directory contains bin/ and lib/ directories that we need.
CUDA_DIR := /usr/local/cuda-8.0
# On Ubuntu 14.04, if cuda tools are installed via
# "sudo apt-get install nvidia-cuda-toolkit" then use this instead:
# CUDA_DIR := /usr
# CUDA architecture setting: going with all of them.
# For CUDA < 6.0, comment the *_50 lines for compatibility.
CUDA_ARCH := -gencode arch=compute_20,code=sm_20 \
        -gencode arch=compute_20,code=sm_21 \
        -gencode arch=compute_30,code=sm_30 \
        -gencode arch=compute_35,code=sm_35 \
        -gencode arch=compute_50,code=sm_50 \
        -gencode arch=compute_50,code=compute_50
# BLAS choice:
# atlas for ATLAS (default)
# mkl for MKL
# open for OpenBlas
BLAS := atlas
# Custom (MKL/ATLAS/OpenBLAS) include and lib directories.
# Leave commented to accept the defaults for your choice of BLAS
# (which should work)!
# BLAS_INCLUDE := /path/to/your/blas
# BLAS_LIB := /path/to/your/blas
# Homebrew puts openblas in a directory that is not on the standard search path
# BLAS_INCLUDE := $(shell brew --prefix openblas)/include
# BLAS_LIB := $(shell brew --prefix openblas)/lib
# This is required only if you will compile the matlab interface.
# MATLAB directory should contain the mex binary in /bin.
MATLAB_DIR := /usr/local/MATLAB/R2013b
# MATLAB_DIR := /Applications/MATLAB_R2012b.app
# NOTE: this is required only if you will compile the python interface.
# We need to be able to find Python.h and numpy/arrayobject.h.
#PYTHON_INCLUDE := /usr/include/python2.7 \
                         /usr/lib/python2.7/dist-packages/numpy/core/include
# Anaconda Python distribution is quite popular. Include path:
# Verify anaconda location, sometimes it's in root.
ANACONDA_HOME := $(HOME)/anaconda2
PYTHON_INCLUDE := $(ANACONDA_HOME)/include \
        $(ANACONDA_HOME)/include/python2.7 \
        $(ANACONDA_HOME)/lib/python2.7/site-packages/numpy/core/include \
# Uncomment to use Python 3 (default is Python 2)
# PYTHON_LIBRARIES := boost_python3 python3.5m
# PYTHON_INCLUDE := /usr/include/python3.5m \
#                 /usr/lib/python3.5/dist-packages/numpy/core/include
# We need to be able to find libpythonX.X.so or .dylib.
# PYTHON_LIB := /usr/lib
PYTHON_LIB := $(ANACONDA_HOME)/lib
# Homebrew installs numpy in a non standard path (keg only)
# PYTHON_INCLUDE += $(dir $(shell python -c 'import numpy.core; print(numpy.core.__file__)'))/include
# PYTHON_LIB += $(shell brew --prefix numpy)/lib
# Uncomment to support layers written in Python (will link against Python libs)
WITH_PYTHON_LAYER := 1
# Whatever else you find you need goes here.
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib
# If Homebrew is installed at a non standard location (for example your home directory) and you use it for general dependencies
# INCLUDE_DIRS += $(shell brew --prefix)/include
# LIBRARY_DIRS += $(shell brew --prefix)/lib
# Uncomment to use `pkg-config` to specify OpenCV library paths.
# (Usually not necessary -- OpenCV libraries are normally installed in one of the above $LIBRARY_DIRS.)
# USE_PKG_CONFIG := 1
# N.B. both build and distribute dirs are cleared on `make clean`
BUILD_DIR := build
DISTRIBUTE_DIR := distribute
# Uncomment for debugging. Does not work on OSX due to https://github.com/BVLC/caffe/issues/171
# DEBUG := 1
# The ID of the GPU that 'make runtest' will use to run unit tests.
TEST_GPUID := 0
# enable pretty build (comment to see full commands)
Q ?= @
```

修改好后，保存退出

4、修改完编译配置文件后，进行编译：

    make all

编译成功后，运行：

    make test -j8
    make runtest -j8

也许在编译runtest的时候，会报这样的错误：

.build_release/test/test_all.testbin: error while loading shared
libraries: libhdf5.so.10: cannot open shared object file: No such file or directory

这是因为libhdf5.so的版本问题，你可以进入/usr/lib/x86_64-linux-gnu看一下，你的libhdf5.so.x中的那个x是多少，比如我的是libhdf5.so.7，因此可以执行下面几行代码解决:

    cd /usr/lib/x86_64-linux-gnu
    sudo ln -s libhdf5.so.7 libhdf5.so.10
    sudo ln -s libhdf5_hl.so.7 libhdf5_hl.so.10
    sudo ldconfig

运行下来没有其他问题，说明编译成功

5、编译python接口

    make pycaffe

编译好后，import caffe不报错则编译成功

6、编译matlab接口

    make matcaffe

编译好后，运行：

    make mattest

进行测试

在caffe根目录打开matlab后，增加路径：

    addpath ./matlab

这样就能在caffe根目录运行matlab文件并加载caffe model了

九、安装Torch（基于lua语言的深度学习框架）

打开终端，执行如下命令，不要使用sudo：

    git clone https://github.com/torch/distro.git ~/torch --recursive
    cd ~/torch; bash install-deps;
    ./install.sh

更新环境变量：

    source ~/.bashrc

如果想要卸载torch，执行:

    rm -rf ~/torch

基础的Torch安装好了后，可以通过Luarocks安装更多的库:

不要使用sudo：

    luarocks install image
    luarocks list
    luarocks install nngraph

安装完成后可以使用th命令运行Torch
