# RobotVision

## Installation steps:

### Dependies:
Eigen 3.1.4
https://bitbucket.org/eigen/eigen/get/3.1.4.tar.gz

```bash
sudo apt-get install git cmake
sudo apt-get install libeigen3-dev libsuitesparse-dev freeglut3-dev libglu-dev libglew-dev libboost-all-dev
```

```bash
mkdir $HOME/svslocal
mkdir $HOME/source
cd $HOME/source

git clone git://github.com/strasdat/ScaViSLAM.git
cd ScaViSLAM

mkdir EXTERNAL
cd EXTERNAL

git clone git://github.com/strasdat/g2o.git
cd g2o
mkdir svs_build
cd svs_build
cmake .. -DCMAKE_INSTALL_PREFIX:PATH=$HOME/svslocal
make -j4
make install
cd ../..
```

