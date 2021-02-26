# Carla ver 0.96 install for UBUNTU 18.04 LTS

##カーネルの確認
uname -srv
(Linux 5.4.0-42-genericだったらok　もし違うのなら
sudo apt install linux-image-5.4.0-42-generic linux-headers-5.4.0-42-generic linux-modules-extra-5.4.0-42-generic
で再起動　Ubuntu起動時にesc押してGrubメニュー表示後、Advanced options for Ubuntu選択、5.4.0-42-generic選択
NVIDIA driver関連で問題が起こるならさらにダウングレードすることも検討)
## pre-requirements
### CUDA & CUDA-toolkit
cd /tmp/  
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-repo-ubuntu1804_10.1.105-1_amd64.deb  
sudo apt install ./cuda-repo-ubuntu1804_10.1.105-1_amd64.deb  
sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub  
sudo apt update  
wget https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/nvidia-machine-learning-repo-ubuntu1804_1.0.0-1_amd64.deb  
sudo apt install ./nvidia-machine-learning-repo-ubuntu1804_1.0.0-1_amd64.deb  
sudo apt update  

### drivers(NVIDIA only)
sudo apt install nvidia-driver-418  
(もしエラーが出たら  
sudo apt install aptitude  
sudo aptitude install nvidia-driver-418　
再起動
これでPC起動しなくなるのであれば、Ubuntuを入れ直し、
sudo ubuntu-drivers autoinstall
再起動)  

nvidia-smi  
(driverのバージョンが418以上ならばok)  


sudo apt install --no-install-recommends cuda-10-1 libcudnn7=7.6.4.38-1+cuda10.1 libcudnn7-dev=7.6.4.38-1+cuda10.1 cuda-drivers=440.64.00-1

(/usr/local/直下にcuda-10.1が入っているか確認)  
(入ってなければcuda-10-1がインストールできていない)  
(入っていたらcuda-10.1/直下にbin/やlib64/が入っているか確認)  
(入ってなければ"sudo apt install cuda-toolkit-10-1"をしてもう一度確認)  

echo 'export CUDA_PATH=/usr/local/cuda-10.1' >> ${HOME}/.bashrc  
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-10.1/lib64:${LD_LIBRARY_PATH}' >> ${HOME}/.bashrc  
echo 'export PATH=/usr/local/cuda-10.1/bin:${PATH}' >> ${HOME}/.bashrc  


### python
sudo apt install python3.7  
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 2  
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 1  
sudo update-alternatives --config python3(python3.6と3.7が入っているか確認してEnter)  

cd /usr/lib/python3/dist-packages  
sudo ln -s apt_pkg.cpython-{36m,37m}-x86_64-linux-gnu.so  
cd /usr/lib/python3/dist-packages/gi  
sudo ln -s _gi.cpython-{36m,37m}-x86_64-linux-gnu.so  

### git
cd ~  
sudo apt install git


## install Carla with Unreal Engine
(refered to https://carla.readthedocs.io/en/0.9.6/how_to_build_on_linux/)  

sudo apt-get update  
sudo apt-get install wget software-properties-common  
sudo add-apt-repository ppa:ubuntu-toolchain-r/test  
wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key|sudo apt-key add -  
sudo apt-add-repository "deb http://apt.llvm.org/xenial/ llvm-toolchain-xenial-7 main"  
sudo apt-get update  
sudo apt-get install build-essential clang-7 lld-7 g++-7 cmake ninja-build libvulkan1 python python-pip python-dev python3-dev python3-pip libpng-dev libtiff5-dev libjpeg-dev tzdata sed curl unzip autoconf libtool rsync   
(libpng16-dev -> libpng-dev)  
sudo apt update  

pip install --user setuptools  
pip3 install --user setuptools  

sudo update-alternatives --install /usr/bin/clang++ clang++ /usr/lib/llvm-7/bin/clang++ 170  
sudo update-alternatives --install /usr/bin/clang clang /usr/lib/llvm-7/bin/clang 170  

git clone --depth=1 -b 4.22 https://github.com/EpicGames/UnrealEngine.git ~/UnrealEngine_4.22  
cd ~/UnrealEngine_4.22  
./Setup.sh && ./GenerateProjectFiles.sh && make  
cd ~/UnrealEngine_4.22/Engine/Binaries/Linux && ./UE4Editor  
(開けたら閉じてok)  

cd ~  
git clone https://github.com/carla-simulator/carla  
cd ~/carla
git checkout e2c4dc1  
(errorが出るけど無視してok)  

sudo apt-get install aria2  

./Update.sh  

export UE4_ROOT=~/UnrealEngine_4.22  

make launch  
make PythonAPI  
make package  
(carla/Dist/内のファイルを確認. ok:CARLA_0.9.6.tar.gz  not:CARLA_0.9.9.tar.gz)  

carlaのディレクトリを２つ作る.(区別が付くように名前をつけること)  
①(作成したパッケージ版)  
CARLA_0.9.6.tar.gzを展開してこのディレクトリに  

②(直接ダウンロードするリリース版)  
https://github.com/carla-simulator/carla/releases/tag/0.9.6  
↑のページからCARLA_0.9.6.tar.gzをダウンロードして展開し保存

②内の(PythonAPI/carla/dist/###-py3.5-egg)を，①内の(PythonAPI/carla/dist/)に，###-py3.6-eggと###-py3.7-eggの２つ名前でコピー  
(cp /PythonAPI/carla/dist/###-py3.5-egg /PythonAPI/carla/dist/###-py3.6-egg ###-py3.7-egg)  
もともと①内の###-py3.x-eggファイルは削除  

sudo rm -fdr /usr/local/lib/python3.7/dist-package/*  
sudo rm -fdr /usr/local/lib/python3.6/dist-package/*  

sudo apt install pkg-config libopenblas-dev liblapack-dev libhdf5-serial-dev graphviz  
sudo python3 -m pip install --upgrade pip  
pip install pygame numpy opencv-python  
pip3 install pygame numpy opencv-python tqdm  


## Run carla
./CarlaUE4.sh -opengl  
(重いときは "-quality-level=Low" のオプションをつける)  
