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
    cd py-faster-rcnn
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
