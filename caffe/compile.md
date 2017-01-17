# Compiling Caffe on Ubuntu

Follow http://caffe.berkeleyvision.org/install_apt.html then

    sudo apt-get install libblas-dev python3-dev
    sudo apt-get install libatlas-base-dev
    sudo apt-get install libatlas-dev
    sudo apt-get install libblas-dev
    conda install boost
    
Correct bug in boost https://github.com/BVLC/caffe/issues/3578:

change suffix.hpp line 512, from 

    __extension__ typedef float128 float128_type; 
to

    __extension__ typedef long double float128_type;
    
Correct for bug with `memcpy` in string.h https://github.com/BVLC/caffe/issues/4046

    edit Makefile and replace the line
    NVCCFLAGS += -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS)
    with
    NVCCFLAGS += -D_FORCE_INLINES -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS).
    
    
    make all -j8
    make pycaffe
    export PYTHONPATH=`pwd`/python:$PYTHONPATH
    echo "export PYTHONPATH=`pwd`/python:\$PYTHONPATH" >> ~/.bashrc
    
    
My `Makefile.config` file is here: https://gist.github.com/groakat/5290ba684db82c7083bbbc2859bef8d6


# Compiling for Python 2.7 (

Make sure that you do **not** have `boost` installed (as it messes up the python bindings and you get a strange error saying the function signitures do not match up when loading a caffe model. If you have `boost` installed, remove it with `conda uninstall boost`. Also do **not** install `protobuf` in anaconda, because it will conflict with your system `protobuf`/ `protoc` and cause another nasty error. (If you installed `protobuf` in anacoda, you **cannot** remove it with `conda uninstall protobuf`. You have to find the related files with `find /home/peter/anacondaX/ -name "*proto*"` and remove them by hand. 


    git clone --recursive https://github.com/rbgirshick/py-faster-rcnn.git
    cd py-faster-rcnn/caffe-fast-rcnn
    git remote add caffe https://github.com/BVLC/caffe.git
    git fetch caffe
    git merge caffe/master
    vim include/caffe/layers/python_layer.hpp
    
(Remove `self_.attr("phase") = static_cast<int>(this->phase_);` from `include/caffe/layers/python_layer.hpp` after merging. as described here: https://github.com/rbgirshick/py-faster-rcnn/issues/237)

    conda install pyyaml python-gflags cython scipy scikit-image ipython h5py leveldv networkx pandas six
    pip install easydict
    sudo apt-get install libprotobuf-dev protobuf-compiler protobuf-c-compiler libprotoc-dev libprotobuf9v5 libprotobuf-lite9v5
    
    
Also you **cannot** build the caffe project in parallel using the `-jX` flag. It will only work once the first file in the make order has been build. So either use `make all` for everything or <kbd>Ctrl</kbd>+<kbd>c</kbd> after the first file has been compiled and then run `make all -j8` afterwards to save time.

    make all
    make all -j8
    make pycaffe
    
Then the demo should run

    cd ..
    ./data/scripts/fetch_faster_rcnn_models.sh
    ./tools/demo.py
    
    
# Compiling for Python 3.5 (anaconda)

    git clone https://github.com/BVLC/caffe.git
    cd caffe
    cp Makefile.config.example Makefile.config
    kate Makefile.config
    
Change `Makefile.config` to

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
    #	You should not set this flag if you will be reading LMDBs with any
    #	possibility of simultaneous read and write
    # ALLOW_LMDB_NOLOCK := 1

    # Uncomment if you're using OpenCV 3
    OPENCV_VERSION := 3

    # To customize your choice of compiler, uncomment and set the following.
    # N.B. the default for Linux is g++ and the default for OSX is clang++
    # CUSTOM_CXX := g++

    # CUDA directory contains bin/ and lib/ directories that we need.
    CUDA_DIR := /usr/local/cuda
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
    # MATLAB_DIR := /usr/local
    # MATLAB_DIR := /Applications/MATLAB_R2012b.app

    # NOTE: this is required only if you will compile the python interface.
    # We need to be able to find Python.h and numpy/arrayobject.h.
    PYTHON_INCLUDE := /usr/include/python3.5 \
            /usr/lib/python3.5/dist-packages/numpy/core/include
    # Anaconda Python distribution is quite popular. Include path:
    # Verify anaconda location, sometimes it's in root.
    ANACONDA_HOME := $(HOME)/anaconda3
    PYTHON_INCLUDE := $(ANACONDA_HOME)/include \
              $(ANACONDA_HOME)/include/python3.5m \
              $(ANACONDA_HOME)/lib/python3.5/site-packages/numpy/core/include \

    # Uncomment to use Python 3 (default is Python 2)
    # PYTHON_LIBRARIES := boost_python3 python3.5m
    # PYTHON_INCLUDE := $(PYTHON_INCLUDE) /usr/include/python3.5m \
      #                /usr/lib/python3.5/dist-packages/numpy/core/include

    # We need to be able to find libpythonX.X.so or .dylib.
    # PYTHON_LIB := /usr/lib
    PYTHON_LIB := $(ANACONDA_HOME)/lib

    # Homebrew installs numpy in a non standard path (keg only)
    # PYTHON_INCLUDE += $(dir $(shell python -c 'import numpy.core; print(numpy.core.__file__)'))/include
    # PYTHON_LIB += $(shell brew --prefix numpy)/lib

    # Uncomment to support layers written in Python (will link against Python libs)
    # WITH_PYTHON_LAYER := 1

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

back in terminal:

    make all -j8
    make pycaffe

