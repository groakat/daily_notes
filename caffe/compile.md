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
    
    
My `Makefile.config` file is here: https://gist.github.com/groakat/5290ba684db82c7083bbbc2859bef8d6
