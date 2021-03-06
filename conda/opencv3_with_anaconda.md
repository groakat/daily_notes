# Installing OpenCV on OSX alongside anaconda

## Create new Environment

I am using `lala` as name for the environment in this example.

    conda create --name lala python=3

## Install opencv3 with homebrew

    brew install homebrew/science/opencv3 --with-contrib --with-ffmpeg --with-python4 --with-opengl --with-qt5 --with-tbb

it might be that it is necessary to install hdf5 separately

    brew install hdf5


To make OpenCV available to anaconda, a Pythonpath has to be set:

    echo /usr/local/opt/opencv3/lib/python3.5/site-packages >> /Users/peter/anaconda/envs/lala/lib/python3.5/site-packages/opencv3.pth

Done.

# Installing OpenCV in Ubuntu 16.04

Generally, follow: http://www.pyimagesearch.com/2015/07/20/install-opencv-3-0-and-python-3-4-on-ubuntu/

Install packages

    sudo apt-get install build-essential cmake git pkg-config libjpeg8-dev libtiff5-dev libjasper-dev libpng12-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libgtk2.0-dev libhdf5-dev
    
Clone openCV

    git clone https://github.com/Itseez/opencv.git
    cd opencv
    git checkout 3.1.0
    cd ..
    
    git clone https://github.com/Itseez/opencv_contrib.git
    cd opencv_contrib
    git checkout 3.1.0
    
    cd ../opencv
    mkdir build
    cd build
    
If to be build with HDF5 support, follow https://github.com/opencv/opencv/issues/6016 to get rid of compiler error. Basically add the two lines 

    find_package(HDF5)
    include_directories(${HDF5_INCLUDE_DIRS})
    
to `opencv/modules/python/common.cmake` (I added them at the top).
  
```bash
cmake -DBUILD_TIFF=ON -DBUILD_opencv_java=OFF -DWITH_CUDA=OFF -DENABLE_AVX=ON -DWITH_OPENGL=ON -DWITH_OPENCL=ON -DWITH_IPP=ON -DWITH_TBB=ON -DWITH_EIGEN=ON -DWITH_V4L=ON -DWITH_VTK=OFF -DBUILD_TESTS=OFF -DBUILD_PERF_TESTS=OFF -DCMAKE_BUILD_TYPE=RELEASE -DCMAKE_INSTALL_PREFIX=$(python3 -c "import sys; print(sys.prefix)") -DPYTHON3_EXECUTABLE=$(which python3) -DPYTHON3_INCLUDE_DIR=$(python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") -DPYTHON3_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") ..
```

Symlink package to anaconda:

    cd ~/anaconda3/lib/python3.5/site-packages/
    ln -s /usr/local/lib/python3.5/site-packages/cv2.cpython-35m-x86_64-linux-gnu.so cv2.so


    
