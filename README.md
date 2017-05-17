Ata Niyazov 130201108
GitHub link'i:
* https://github.com/ataniazov/RobotVision
---

# RobotVision (ScaViSLAM)

Bu görsel SLAM için genel ve ölçeklenebilir bir kütüphane. Bu kütüphane ICCV belgesinde anlatılan "Çift Görüntü Optimizasyonu" (Double Window Optimization) yöntemleri gerçeklemektedir:
* H. Strasdat, A.J. Davison, J.M.M. Montiel, and K. Konolige 
"Double Window Optimisation for Constant Time Visual SLAM" 
Proceedings of the IEEE International Conference on Computer Vision, 2011.

Şu anda kalibre edilmiş stereo görüntü verileri ve RGB-D kameraları destekliyor.
Monoküler SLAM desteklenmiyor (henüz).

Bu kütüphane geliştirme aşamasındaki araştırma düzeyinde bir yazılımdır.
Bu bir piyasada kullanılabilecek sürüm değildir. Herhalde özellikler eklenecek,
hatalar giderilecek ve API ileride değişecektir.

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
* http://www.robots.ox.ac.uk/NewCollegeData/index.php?n=Main.Downloads#Full


## Kurulum adımları

1. Bağımlılıkları yükleyin:
```bash
sudo apt-get install git cmake gcc-4.6 gcc-4.6-multilib g++-4.6 g++-4.6-multilib libstdc++6-4.6-dev qt4-qmake qt4-default qt4-dev-tools
sudo apt-get install libeigen3-dev libsuitesparse-dev freeglut3-dev libglu-dev libglew-dev libboost-all-dev
```

2. Varsayılan gcc ve g++ derleyicisini 4.6 yapınız:
```bash
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.6 10
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.6 10
sudo update-alternatives --set cc /usr/bin/gcc
sudo update-alternatives --set c++ /usr/bin/g++
```

3. Sürüm çakışmalarını önlemek için kütüphanelerin hepsini yerel olarak ev dizininizde yüklemenizi öneririm, sistem genelinde değil.

```bash
mkdir $HOME/svslocal
```

4. ScaViSLAM kaynak kodlarını GitHub havuzundan indirin/klonlayın.

```bash
mkdir $HOME/source
cd $HOME/source

git clone git://github.com/strasdat/ScaViSLAM.git
cd ScaViSLAM

mkdir EXTERNAL
cd EXTERNAL
```

5. g2o'ni indirin:

```bash
git clone git://github.com/strasdat/g2o.git
cd g2o
```

6. İşlemciniz Intel Core i7/i5/i3 ise CMakeLists.txt aşagıdaki ayarı yapınız:
```bash
sed -i '/-march=/s/native/corei7-avx/g' CMakeLists.txt
```

7. Ardından derleme ve kurma işlemini yapınız:

```bash
mkdir svs_build
cd svs_build
cmake .. -DCMAKE_INSTALL_PREFIX:PATH=$HOME/svslocal
make -j4
make install
cd ../..
```

8. EXTERNAL dizinine opencv (version 2.4.13.2) kütüphanesini indirin ve kurun:

```bash
wget https://github.com/opencv/opencv/archive/2.4.13.2.tar.gz
tar -xvzf 2.4.13.2.tar.gz
cd opencv-2.4.13.2/
mkdir svs_build
cd svs_build
cmake .. -DCMAKE_INSTALL_PREFIX:PATH=$HOME/svslocal
make -j4
make install
cd ../..
```

9. EXTERNAL dizinine Pangolin'i kopyalayın ve kurun:

```bash
git  clone git://github.com/strasdat/Pangolin.git
cd Pangolin
git checkout 7ecfa4c
mkdir svs_build
cd svs_build
cmake .. -DBUILD_SHARED_LIBS=ON -DCMAKE_INSTALL_PREFIX:PATH=$HOME/svslocal
make -j4
make install
cd ../..
```

10. EXTERNAL dizinine Sophus'u kopyalayın ve kurun:

```bash
git clone git://github.com/strasdat/Sophus.git
cd Sophus
git checkout a621ff
mkdir svs_build
cd svs_build
cmake .. -DCMAKE_INSTALL_PREFIX:PATH=$HOME/svslocal
make -j4
make install
cd ../..
```

11. EXTERNAL dizinine VisionTools'u kopyalayın ve kurun:

```bash
git clone git://github.com/strasdat/VisionTools.git
cd VisionTools
mkdir svs_build
cd svs_build
cmake .. -DCMAKE_PREFIX_PATH:PATH=$HOME/svslocal -DCMAKE_INSTALL_PREFIX:PATH=$HOME/svslocal
make -j4
make install
```

12. "ScaViSLAM" ana proje klasörüne gidin:

```bash
cd ../../../
```

13. ScaViSLAM'i CUDA desteği olmaksızın derlemek isterseniz, CMakeLists.txt cmake ayarlama dosyasında CUDA_SUPPORT OFF olarak değiştirin.

```bash
sed -i '/SET(CUDA_SUPPORT /s/ON)/OFF)/g' CMakeLists.txt
```

14. Boost kütüphanesinin eski "filesystem3" namespace'ini yeni "filesystem" namespace'i ile değiştiriniz:

```bash
sed -i '/boost::/s/filesystem3/filesystem/g' scavislam/create_dictionary.cpp
```

15. ScaViSLAM'i kurun:

```bash
mkdir svs_build
cd svs_build
cmake .. -DCMAKE_PREFIX_PATH:PATH=$HOME/svslocal
sed -i.bck '$s/$/-lGLU \/usr\/lib\/x86_64-linux-gnu\/libGLU.so.1/' CMakeFiles/stereo_slam.dir/link.txt
make -j4
```

16. "New College" veri setinin tamamen indirildiğinden emin olun.
 Ardından, "ScaViSLAM / data" içinde bir simge bağlantısı ekleyin:

```bash
cd ../data
ln -s PATH_TO_MY_DATA_DIRECTORY/newcollege/(subdirectory) newcollege
```

17. Burada, alt dizin görüntü sırası dizini içeriyor. Daha uzun bir sıralama yapmak isterseniz, dosyaları tek dizin olarak bir arada birleştirin.

18. "New College" resim dizisinde ScaViSLAM'ı çalıştırın:

```bash
cd ../svs_build
./stereo_slam ../data/newcollege.cfg
```

İlgili videolara göz atın:
* http://youtu.be/90Rw3qDuWrw
* http://youtu.be/89CNVQ5azsc

**Kaynakça:**
1. https://github.com/strasdat/ScaViSLAM
2. H. Strasdat, A.J. Davison, J.M.M. Montiel, and K. Konolige 
"Double Window Optimisation for Constant Time Visual SLAM" 
Proceedings of the IEEE International Conference on Computer Vision, 2011.
