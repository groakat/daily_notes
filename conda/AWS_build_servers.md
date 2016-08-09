# Setting up and using build servers in AWS

## Windows

### Basic installation (incl. SSH server)
Follow guide here: http://petemoore.github.io/general/taskcluster/2016/03/30/windows-sshd-cygwin-ec2-aws.html

### Login

  ssh -i '1470649420.pem' Administrator@52.50.213.43

### Install apt-cyg

1. install `apt-cyg`: https://github.com/transcode-open/apt-cyg

  lynx -source rawgit.com/transcode-open/apt-cyg/master/apt-cyg > apt-cyg
  install apt-cyg /bin
  
2. install git

  apt-cyg install git
  
### Install Anaconda

1. Download installer https://www.continuum.io/downloads
2. `cmd`
3. start /wait "" Anaconda2-4.1.1-Windows-x86_64.exe /InstallationType=JustMe /RegisterPython=1 /S /D=C:\cygwin\home\Administrator\Anaconda2
4. reboot instance (to register python)


### Update Conda

  conda upgrade conda-build
  conda upgrade anaconda-client

### Clone and setup anaconda

    cmd
    cd PATH
    cond config --add channels groakat
  
### Build

    conda build videotagger
    anaconda upload C:\cygwin\home\Administrator\Anaconda2\conda-bld\win-64\videotagger-0.1.1-1.tar.bz2
  
If that fails:

    exit
    scp -i '1470649420.pem' Administrator@52.50.213.43:/home/Administrator/Anaconda2/conda-bld/win-64/videotagger-0.1.1-2.tar.bz2 .
    anaconda upload videotagger-0.1.1-2.tar.bz2
  
  
  
