# Install Tensorflow on AWS

## Without CUDA

### Install Anaconda

```bash
cd ~/Downloads
wget http://repo.continuum.io/archive/Anaconda3-4.1.1-Linux-x86_64.sh
bash Anaconda3-4.1.1-Linux-x86_64.sh -b -p $HOME/anaconda3

echo '#Anaconda
PATH="$HOME/anaconda3/bin:$PATH"' >> ~/.bashrc
```

### Install Tensorflow

```bash
conda create -y -n tensorflow python=3.5
source activate tensorflow
conda install -c conda-forge tensorflow
```

### Install other packages

```bash
conda install -y ipython ipython-notebook pandas numpy matplotlib
