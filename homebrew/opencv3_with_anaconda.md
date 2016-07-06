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
