# RobotVision (ScaViSLAM)

İşlemler yapılacak işletim sistemi özellikleri:

```bash
lsb_release -a && uname -a

No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 14.04.5 LTS
Release:	14.04
Codename:	trusty
Linux ubuntu-14.04.5 4.4.0-75-generic #96~14.04.1-Ubuntu SMP Thu Apr 20 11:06:30 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
```

## Veri seti (Dataset)

Önce örnek resim veri setini indirin. Bu biraz zaman alacak, bu yüzden önce yapınız:

```bash
wget http://www.robots.ox.ac.uk/NewCollegeData/Data/FullData/StereoImages/StereoImages_1225720041.455302_to_1225720118.251935.tgz
wget http://www.robots.ox.ac.uk/NewCollegeData/Data/FullData/StereoImages/StereoImages_1225720118.301927_to_1225720193.248630.tgz 
wget http://www.robots.ox.ac.uk/NewCollegeData/Data/FullData/StereoImages/StereoImages_1225720193.298630_to_1225720268.945303.tgz

mkdir newcollege
find . -name "StereoImages*.tgz" -exec tar xvzf '{}' --strip-components=1 -C newcollege/ \;
```
Verilerin daha büyük bir setini indirmek isterseniz, adresindeki dizine bakın:
http://www.robots.ox.ac.uk/NewCollegeData/index.php?n=Main.Downloads#Full


## Kurulum adımları

1. Bağımlılıkları yükleyin:
```bash
sudo apt-get install git cmake gcc-4.6 gcc-4.6-multilib g++-4.6 g++-4.6-multilib libstdc++6-4.6-dev qt4-qmake qt4-default qt4-dev-tools
sudo apt-get install libeigen3-dev libsuitesparse-dev freeglut3-dev libglu-dev libglew-dev libboost-all-dev
```

2. Sürüm çakışmalarını önlemek için kütüphanelerin hepsini yerel olarak ev dizininizde yüklemenizi öneririm, sistem genelinde değil.

```bash
mkdir $HOME/svslocal
mkdir $HOME/source
cd $HOME/source

git clone git://github.com/strasdat/ScaViSLAM.git
cd ScaViSLAM

mkdir EXTERNAL
cd EXTERNAL
```

Kütüphane g2o:

```bash
git clone git://github.com/strasdat/g2o.git
cd g2o
```

İşlemciniz Intel Core i7,i5,i3 ise:
```bash
sed -i  '/-march=/s/native/corei7-avx/g' CMakeLists.txt
```

mkdir svs_build
cd svs_build
cmake .. -DCMAKE_INSTALL_PREFIX:PATH=$HOME/svslocal
make -j4
make install
cd ../..
```

EXTERNAL dizininin içine opencv (version 2.4.13.2) kütüphanesini indirin ve kurun:

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

İlgili videolara göz atın:
http://youtu.be/90Rw3qDuWrw
http://youtu.be/89CNVQ5azsc

**Kaynakça:**
1. https://github.com/strasdat/ScaViSLAM
2. H. Strasdat, A.J. Davison, J.M.M. Montiel, and K. Konolige 
"Double Window Optimisation for Constant Time Visual SLAM" 
Proceedings of the IEEE International Conference on Computer Vision, 2011.
