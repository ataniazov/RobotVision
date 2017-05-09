# RobotVision

```bash
lsb_release -a && uname -a

No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 14.04.5 LTS
Release:	14.04
Codename:	trusty
Linux skynet 4.4.0-75-generic #96~14.04.1-Ubuntu SMP Thu Apr 20 11:06:30 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
```

## DataSet
```bash
wget http://www.robots.ox.ac.uk/NewCollegeData/Data/FullData/StereoImages/StereoImages_1225720041.455302_to_1225720118.251935.tgz
wget http://www.robots.ox.ac.uk/NewCollegeData/Data/FullData/StereoImages/StereoImages_1225720118.301927_to_1225720193.248630.tgz 
wget http://www.robots.ox.ac.uk/NewCollegeData/Data/FullData/StereoImages/StereoImages_1225720193.298630_to_1225720268.945303.tgz
```

## Installation steps:

### Dependies:
Eigen 3.1.4
https://bitbucket.org/eigen/eigen/get/3.1.4.tar.gz

```bash
sudo apt-get install libeigen3-dev libsuitesparse-dev freeglut3-dev libglu-dev libglew-dev libboost-all-dev

sudo apt-get install git cmake g++
```

```bash
mkdir $HOME/svslocal
mkdir $HOME/source
cd $HOME/source

git clone git://github.com/strasdat/ScaViSLAM.git
cd ScaViSLAM

mkdir EXTERNAL
cd EXTERNAL
```

Check out and install g2o:

```bash
git clone git://github.com/strasdat/g2o.git
cd g2o
mkdir svs_build
cd svs_build
cmake .. -DCMAKE_INSTALL_PREFIX:PATH=$HOME/svslocal
make -j4
make install
cd ../..
```

Download and install opencv (version 2.4.13.2) inside EXTERNAL:

```
wget https://github.com/opencv/opencv/archive/2.4.13.2.tar.gz
tar -xvzf opencv-2.4.13.2.tar.gz
cd opencv-2.4.13.2/
mkdir svs_build
cd svs_build
cmake .. -DCMAKE_INSTALL_PREFIX:PATH=$HOME/svslocal
make -j4
make install
cd ../..
```


