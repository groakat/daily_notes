# Install CUDA
- [AWS Instructions here](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using_cluster_computing.html#install-nvidia-driver)
- [Complete Tutorial here](http://www.pyimagesearch.com/2016/07/04/how-to-install-cuda-toolkit-and-cudnn-for-deep-learning/)

```bash 

    sudo apt-get update
    sudo apt-get upgrade -y
    sudo apt-get install -y build-essential cmake git unzip pkg-config
    sudo apt-get install -y libopenblas-dev liblapack-dev
    sudo apt-get install -y linux-image-generic linux-image-extra-virtual
    sudo apt-get install -y linux-source linux-headers-generic
    
    
    sudo sh -c 'echo "blacklist nouveau
    blacklist lbm-nouveau
    options nouveau modeset=0
    alias nouveau off
    alias lbm-nouveau off" >> /etc/modprobe.d/blacklist-nouveau.conf'
    
    echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
    sudo update-initramfs -u
    sudo reboot
    
    # INTERACTION
    
    sudo /bin/bash NVIDIA-Linux-x86_64-367.35.
    
    # INTERACTION
    
    modprobe nvidia
    
    wget http://developer.download.nvidia.com/compute/cuda/7.5/Prod/local_installers/cuda_7.5.18_linux.run
    mkdir installers
    chmod +x cuda_7.5.18_linux.run
    
    # INTERACTION
    
    sudo ./cuda_7.5.18_linux.run -extract=`pwd`/installers
    cd installers
    
    sudo ./cuda-linux64-rel-7.5.18-19867135.run
    
    # INTERACTION
    
    sudo ./cuda-samples-linux-7.5.18-19867135.run
    
    # INTERACTION
    
    echo "# CUDA Toolkit
    export CUDA_HOME=/usr/local/cuda-7.5
    export LD_LIBRARY_PATH=${CUDA_HOME}/lib64:$LD_LIBRARY_PATH
    export PATH=${CUDA_HOME}/bin:${PATH}" >> ~/.bashrc
    
    exit
    scp -i faster-rcnn.pem cudnn-7.5-linux-x64-v5.1-rc.tgz ubuntu@52.209.83.104:~/Downloads/
    ./connect
    cd Downloads/
    tar -zxf cudnn-7.5-linux-x64-v5.1-rc.tgz
    cd cuda
    sudo cp lib64/* /usr/local/cuda/lib64/
    sudo cp include/* /usr/local/cuda/include/
```


# Install Anaconda
