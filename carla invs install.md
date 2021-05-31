# Carla INVS install ubuntu18.04
https://github.com/zijianzhang/CARLA_INVS 　をインストール
  

### carla 0.9.8 install

sudo apt-get update &&  

sudo apt-get install wget software-properties-common  

sudo add-apt-repository ppa:ubuntu-toolchain-r/test  

wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key|sudo apt-key add -  

sudo apt-add-repository "deb http://apt.llvm.org/xenial/ llvm-toolchain-xenial-7 main"  

sudo apt-get update  

  

sudo apt-get install build-essential clang-7 lld-7 g++-7 cmake ninja-build libvulkan1 python python-pip python-dev python3-dev python3-pip libpng-dev libtiff5-dev libjpeg-dev tzdata sed curl unzip autoconf libtool rsync libxml2-dev &&  

pip2 install --user setuptools &&  

pip3 install --user setuptools  

  

sudo update-alternatives --install /usr/bin/clang++ clang++ /usr/lib/llvm-7/bin/clang++ 170 &&

sudo update-alternatives --install /usr/bin/clang clang /usr/lib/llvm-7/bin/clang 170

  

git clone --depth=1 -b 4.22 https://github.com/EpicGames/UnrealEngine.git ~/UnrealEngine_4.22

cd ~/UnrealEngine_4.22

./Setup.sh && ./GenerateProjectFiles.sh && make

cd ~/UnrealEngine_4.22/Engine/Binaries/Linux && ./UE4Editor

(開けたら成功)

  

git clone https://github.com/carla-simulator/carla

cd ~/carla

git checkout e9dd27f

./Update.sh

export UE4_ROOT=~/UnrealEngine_4.22

  

make launch

make PythonAPI

make package

  

### python3.7

anacondaをいれてpython3.7を指定して仮想環境作成

(invs) atusi@atusi-G3-3500:~$ python3

Python 3.7.10 (default, Feb 26 2021, 18:47:35)

[GCC 7.3.0] :: Anaconda, Inc. on linux

Type "help", "copyright", "credits" or "license" for more information.

  

### CUDA10.1

(invs) atusi@atusi-G3-3500:~$ cat /usr/local/cuda/version.txt

CUDA Version 10.1.243

  

### pytorch1.4.0

https://pytorch.org/get-started/previous-versions/  から以下のコマンドで入手

conda install pytorch==1.4.0 torchvision==0.5.0 cudatoolkit=10.1 -c pytorch

  

(invs) atusi@atusi-G3-3500:~$ python3

Python 3.7.10 (default, Feb 26 2021, 18:47:35)

[GCC 7.3.0] :: Anaconda, Inc. on linux

Type "help", "copyright", "credits" or "license" for more information.

\>>> import torch

\>>> print(torch.__version__)

1.4.0

>>>

  

### llvm10.0

https://apt.llvm.org/ を参考に

wget https://apt.llvm.org/llvm.sh

chmod +x llvm.sh

sudo ./llvm.s 10

  

その後./bashrcに以下を追記

export PATH=/usr/lib/llvm-10/bin:$PATH

export LD_LIBRARY_PATH=/usr/lib/llvm-10/lib:$LD_LIBRARY_PATH  

  

(invs) atusi@atusi-G3-3500:~$ clang -v

Ubuntu clang version 10.0.1-++20210405103842+ef32c611aa21-1~exp1~20210405084441.211

Target: x86_64-pc-linux-gnu

Thread model: posix

InstalledDir: /usr/lib/llvm-10/bin

Found candidate GCC installation: /usr/lib/gcc/x86_64-linux-gnu/5

Found candidate GCC installation: /usr/lib/gcc/x86_64-linux-gnu/5.5.0

Found candidate GCC installation: /usr/lib/gcc/x86_64-linux-gnu/7

Found candidate GCC installation: /usr/lib/gcc/x86_64-linux-gnu/7.5.0

Found candidate GCC installation: /usr/lib/gcc/x86_64-linux-gnu/8

Selected GCC installation: /usr/lib/gcc/x86_64-linux-gnu/7.5.0

Candidate multilib: .;@m64

Selected multilib: .;@m64

Found CUDA installation: /usr/local/cuda-10.1, version 10.1


  

### carla_invs install

https://github.com/zijianzhang/CARLA_INVS

make dependency　をおこなうと以下のエラー

  

(invs) atusi@atusi-G3-3500:~/CARLA_INVS$ make dependency


pip3 install -r requirement.txt


Requirement already satisfied: open3d>=0.10 in /home/atusi/.local/lib/python3.7/site-packages (from -r requirement.txt (line 1)) (0.11.2)


Requirement already satisfied: plyfile in /home/atusi/.local/lib/python3.7/site-packages (from -r requirement.txt (line 2)) (0.7.4)


Requirement already satisfied: notebook in /home/atusi/.local/lib/python3.7/site-packages (from -r requirement.txt (line 3)) (6.4.0)


Requirement already satisfied: numpy in /home/atusi/.local/lib/python3.7/site-packages (from -r requirement.txt (line 4)) (1.20.3)


Requirement already satisfied: sklearn in /home/atusi/.local/lib/python3.7/site-packages (from -r requirement.txt (line 5)) (0.0)


Requirement already satisfied: matplotlib in /home/atusi/.local/lib/python3.7/site-packages (from -r requirement.txt (line 6)) (3.4.2)


Requirement already satisfied: shapely in /home/atusi/.local/lib/python3.7/site-packages (from -r requirement.txt (line 7)) (1.7.1)


Requirement already satisfied: lxml>=4.5.0 in /home/atusi/.local/lib/python3.7/site-packages (from -r requirement.txt (line 8)) (4.6.3)


Requirement already satisfied: halo in /home/atusi/.local/lib/python3.7/site-packages (from -r requirement.txt (line 9)) (0.0.31)


Requirement already satisfied: picotui in /home/atusi/.local/lib/python3.7/site-packages (from -r requirement.txt (line 10)) (1.2)


Requirement already satisfied: widgetsnbextension in /home/atusi/.local/lib/python3.7/site-packages (from open3d>=0.10->-r requirement.txt (line 1)) (3.5.1)


Requirement already satisfied: addict in /home/atusi/.local/lib/python3.7/site-packages (from open3d>=0.10->-r requirement.txt (line 1)) (2.4.0)


Requirement already satisfied: pandas in /home/atusi/.local/lib/python3.7/site-packages (from open3d>=0.10->-r requirement.txt (line 1)) (1.2.4)


Requirement already satisfied: ipywidgets in /home/atusi/.local/lib/python3.7/site-packages (from open3d>=0.10->-r requirement.txt (line 1)) (7.6.3)


Requirement already satisfied: tqdm in /home/atusi/.local/lib/python3.7/site-packages (from open3d>=0.10->-r requirement.txt (line 1)) (4.61.0)


Requirement already satisfied: pyyaml in /home/atusi/.local/lib/python3.7/site-packages (from open3d>=0.10->-r requirement.txt (line 1)) (5.4.1)


Requirement already satisfied: tornado>=6.1 in /home/atusi/.local/lib/python3.7/site-packages (from notebook->-r requirement.txt (line 3)) (6.1)


Requirement already satisfied: pyzmq>=17 in /home/atusi/.local/lib/python3.7/site-packages (from notebook->-r requirement.txt (line 3)) (22.1.0)


Requirement already satisfied: terminado>=0.8.3 in /home/atusi/.local/lib/python3.7/site-packages (from notebook->-r requirement.txt (line 3)) (0.10.0)


Requirement already satisfied: jupyter-client>=5.3.4 in /home/atusi/.local/lib/python3.7/site-packages (from notebook->-r requirement.txt (line 3)) (6.2.0)


Requirement already satisfied: nbformat in /home/atusi/.local/lib/python3.7/site-packages (from notebook->-r requirement.txt (line 3)) (5.1.3)


Requirement already satisfied: prometheus-client in /home/atusi/.local/lib/python3.7/site-packages (from notebook->-r requirement.txt (line 3)) (0.10.1)


Requirement already satisfied: Send2Trash>=1.5.0 in /home/atusi/.local/lib/python3.7/site-packages (from notebook->-r requirement.txt (line 3)) (1.5.0)


Requirement already satisfied: ipykernel in /home/atusi/.local/lib/python3.7/site-packages (from notebook->-r requirement.txt (line 3)) (5.5.5)


Requirement already satisfied: jupyter-core>=4.6.1 in /home/atusi/.local/lib/python3.7/site-packages (from notebook->-r requirement.txt (line 3)) (4.7.1)


Requirement already satisfied: ipython-genutils in /home/atusi/.local/lib/python3.7/site-packages (from notebook->-r requirement.txt (line 3)) (0.2.0)


Requirement already satisfied: nbconvert in /home/atusi/.local/lib/python3.7/site-packages (from notebook->-r requirement.txt (line 3)) (6.0.7)


Requirement already satisfied: argon2-cffi in /home/atusi/.local/lib/python3.7/site-packages (from notebook->-r requirement.txt (line 3)) (20.1.0)


Requirement already satisfied: jinja2 in /home/atusi/.local/lib/python3.7/site-packages (from notebook->-r requirement.txt (line 3)) (3.0.1)


Requirement already satisfied: traitlets>=4.2.1 in /home/atusi/.local/lib/python3.7/site-packages (from notebook->-r requirement.txt (line 3)) (5.0.5)


Requirement already satisfied: nest-asyncio>=1.5 in /home/atusi/.local/lib/python3.7/site-packages (from jupyter-client>=5.3.4->notebook->-r requirement.txt (line 3)) (1.5.1)


Requirement already satisfied: python-dateutil>=2.1 in /home/atusi/.local/lib/python3.7/site-packages (from jupyter-client>=5.3.4->notebook->-r requirement.txt (line 3)) (2.8.1)


Requirement already satisfied: six>=1.5 in /home/atusi/.local/lib/python3.7/site-packages (from python-dateutil>=2.1->jupyter-client>=5.3.4->notebook->-r requirement.txt (line 3)) (1.16.0)


Requirement already satisfied: ptyprocess in /home/atusi/.local/lib/python3.7/site-packages (from terminado>=0.8.3->notebook->-r requirement.txt (line 3)) (0.7.0)


Requirement already satisfied: scikit-learn in /home/atusi/.local/lib/python3.7/site-packages (from sklearn->-r requirement.txt (line 5)) (0.24.2)


Requirement already satisfied: kiwisolver>=1.0.1 in /home/atusi/.local/lib/python3.7/site-packages (from matplotlib->-r requirement.txt (line 6)) (1.3.1)


Requirement already satisfied: pillow>=6.2.0 in /home/atusi/.local/lib/python3.7/site-packages (from matplotlib->-r requirement.txt (line 6)) (8.2.0)


Requirement already satisfied: pyparsing>=2.2.1 in /home/atusi/.local/lib/python3.7/site-packages (from matplotlib->-r requirement.txt (line 6)) (2.4.7)


Requirement already satisfied: cycler>=0.10 in /home/atusi/.local/lib/python3.7/site-packages (from matplotlib->-r requirement.txt (line 6)) (0.10.0)


Requirement already satisfied: log-symbols>=0.0.14 in /home/atusi/.local/lib/python3.7/site-packages (from halo->-r requirement.txt (line 9)) (0.0.14)


Requirement already satisfied: termcolor>=1.1.0 in /home/atusi/.local/lib/python3.7/site-packages (from halo->-r requirement.txt (line 9)) (1.1.0)


Requirement already satisfied: colorama>=0.3.9 in /home/atusi/.local/lib/python3.7/site-packages (from halo->-r requirement.txt (line 9)) (0.4.4)


Requirement already satisfied: spinners>=0.0.24 in /home/atusi/.local/lib/python3.7/site-packages (from halo->-r requirement.txt (line 9)) (0.0.24)


Requirement already satisfied: cffi>=1.0.0 in /home/atusi/.local/lib/python3.7/site-packages (from argon2-cffi->notebook->-r requirement.txt (line 3)) (1.14.5)


Requirement already satisfied: pycparser in /home/atusi/.local/lib/python3.7/site-packages (from cffi>=1.0.0->argon2-cffi->notebook->-r requirement.txt (line 3)) (2.20)


Requirement already satisfied: ipython>=5.0.0 in /home/atusi/.local/lib/python3.7/site-packages (from ipykernel->notebook->-r requirement.txt (line 3)) (7.24.0)


Requirement already satisfied: pygments in /home/atusi/.local/lib/python3.7/site-packages (from ipython>=5.0.0->ipykernel->notebook->-r requirement.txt (line 3)) (2.9.0)


Requirement already satisfied: jedi>=0.16 in /home/atusi/.local/lib/python3.7/site-packages (from ipython>=5.0.0->ipykernel->notebook->-r requirement.txt (line 3)) (0.18.0)


Requirement already satisfied: pickleshare in /home/atusi/.local/lib/python3.7/site-packages (from ipython>=5.0.0->ipykernel->notebook->-r requirement.txt (line 3)) (0.7.5)


Requirement already satisfied: decorator in /home/atusi/.local/lib/python3.7/site-packages (from ipython>=5.0.0->ipykernel->notebook->-r requirement.txt (line 3)) (5.0.9)


Requirement already satisfied: pexpect>4.3 in /home/atusi/.local/lib/python3.7/site-packages (from ipython>=5.0.0->ipykernel->notebook->-r requirement.txt (line 3)) (4.8.0)


Requirement already satisfied: backcall in /home/atusi/.local/lib/python3.7/site-packages (from ipython>=5.0.0->ipykernel->notebook->-r requirement.txt (line 3)) (0.2.0)


Requirement already satisfied: matplotlib-inline in /home/atusi/.local/lib/python3.7/site-packages (from ipython>=5.0.0->ipykernel->notebook->-r requirement.txt (line 3)) (0.1.2)


Requirement already satisfied: setuptools>=18.5 in /home/atusi/.local/lib/python3.7/site-packages (from ipython>=5.0.0->ipykernel->notebook->-r requirement.txt (line 3)) (57.0.0)


Requirement already satisfied: prompt-toolkit!=3.0.0,!=3.0.1,<3.1.0,>=2.0.0 in /home/atusi/.local/lib/python3.7/site-packages (from ipython>=5.0.0->ipykernel->notebook->-r requirement.txt (line 3)) (3.0.18)


Requirement already satisfied: parso<0.9.0,>=0.8.0 in /home/atusi/.local/lib/python3.7/site-packages (from jedi>=0.16->ipython>=5.0.0->ipykernel->notebook->-r requirement.txt (line 3)) (0.8.2)


Requirement already satisfied: wcwidth in /home/atusi/.local/lib/python3.7/site-packages (from prompt-toolkit!=3.0.0,!=3.0.1,<3.1.0,>=2.0.0->ipython>=5.0.0->ipykernel->notebook->-r requirement.txt (line 3)) (0.2.5)


Requirement already satisfied: jupyterlab-widgets>=1.0.0 in /home/atusi/.local/lib/python3.7/site-packages (from ipywidgets->open3d>=0.10->-r requirement.txt (line 1)) (1.0.0)


Requirement already satisfied: jsonschema!=2.5.0,>=2.4 in /home/atusi/.local/lib/python3.7/site-packages (from nbformat->notebook->-r requirement.txt (line 3)) (3.2.0)


Requirement already satisfied: attrs>=17.4.0 in /home/atusi/.local/lib/python3.7/site-packages (from jsonschema!=2.5.0,>=2.4->nbformat->notebook->-r requirement.txt (line 3)) (21.2.0)


Requirement already satisfied: importlib-metadata in /home/atusi/.local/lib/python3.7/site-packages (from jsonschema!=2.5.0,>=2.4->nbformat->notebook->-r requirement.txt (line 3)) (4.3.1)


Requirement already satisfied: pyrsistent>=0.14.0 in /home/atusi/.local/lib/python3.7/site-packages (from jsonschema!=2.5.0,>=2.4->nbformat->notebook->-r requirement.txt (line 3)) (0.17.3)


Requirement already satisfied: zipp>=0.5 in /home/atusi/.local/lib/python3.7/site-packages (from importlib-metadata->jsonschema!=2.5.0,>=2.4->nbformat->notebook->-r requirement.txt (line 3)) (3.4.1)


Requirement already satisfied: typing-extensions>=3.6.4 in /home/atusi/.local/lib/python3.7/site-packages (from importlib-metadata->jsonschema!=2.5.0,>=2.4->nbformat->notebook->-r requirement.txt (line 3)) (3.10.0.0)


Requirement already satisfied: MarkupSafe>=2.0 in /home/atusi/.local/lib/python3.7/site-packages (from jinja2->notebook->-r requirement.txt (line 3)) (2.0.1)


Requirement already satisfied: defusedxml in /home/atusi/.local/lib/python3.7/site-packages (from nbconvert->notebook->-r requirement.txt (line 3)) (0.7.1)


Requirement already satisfied: entrypoints>=0.2.2 in /home/atusi/.local/lib/python3.7/site-packages (from nbconvert->notebook->-r requirement.txt (line 3)) (0.3)


Requirement already satisfied: mistune<2,>=0.8.1 in /home/atusi/.local/lib/python3.7/site-packages (from nbconvert->notebook->-r requirement.txt (line 3)) (0.8.4)


Requirement already satisfied: bleach in /home/atusi/.local/lib/python3.7/site-packages (from nbconvert->notebook->-r requirement.txt (line 3)) (3.3.0)


Requirement already satisfied: jupyterlab-pygments in /home/atusi/.local/lib/python3.7/site-packages (from nbconvert->notebook->-r requirement.txt (line 3)) (0.1.2)


Requirement already satisfied: pandocfilters>=1.4.1 in /home/atusi/.local/lib/python3.7/site-packages (from nbconvert->notebook->-r requirement.txt (line 3)) (1.4.3)


Requirement already satisfied: nbclient<0.6.0,>=0.5.0 in /home/atusi/.local/lib/python3.7/site-packages (from nbconvert->notebook->-r requirement.txt (line 3)) (0.5.3)


Requirement already satisfied: testpath in /home/atusi/.local/lib/python3.7/site-packages (from nbconvert->notebook->-r requirement.txt (line 3)) (0.5.0)


Requirement already satisfied: async-generator in /home/atusi/.local/lib/python3.7/site-packages (from nbclient<0.6.0,>=0.5.0->nbconvert->notebook->-r requirement.txt (line 3)) (1.10)


Requirement already satisfied: webencodings in /home/atusi/.local/lib/python3.7/site-packages (from bleach->nbconvert->notebook->-r requirement.txt (line 3)) (0.5.1)


Requirement already satisfied: packaging in /home/atusi/.local/lib/python3.7/site-packages (from bleach->nbconvert->notebook->-r requirement.txt (line 3)) (20.9)


Requirement already satisfied: pytz>=2017.3 in /home/atusi/.local/lib/python3.7/site-packages (from pandas->open3d>=0.10->-r requirement.txt (line 1)) (2021.1)


Requirement already satisfied: joblib>=0.11 in /home/atusi/.local/lib/python3.7/site-packages (from scikit-learn->sklearn->-r requirement.txt (line 5)) (1.0.1)


Requirement already satisfied: threadpoolctl>=2.0.0 in /home/atusi/.local/lib/python3.7/site-packages (from scikit-learn->sklearn->-r requirement.txt (line 5)) (2.1.0)


Requirement already satisfied: scipy>=0.19.1 in /home/atusi/.local/lib/python3.7/site-packages (from scikit-learn->sklearn->-r requirement.txt (line 5)) (1.6.3)


sudo apt install libxerces-c3.2 libjpeg8


パッケージリストを読み込んでいます... 完了


依存関係ツリーを作成しています                


状態情報を読み取っています... 完了


libjpeg8 はすでに最新バージョン (8c-2ubuntu8) です。


libxerces-c3.2 はすでに最新バージョン (3.2.0+debian-2) です。


以下のパッケージが自動でインストールされましたが、もう必要とされていません:


  liblldb-10 libnvidia-common-465 linux-image-5.0.0-23-generic linux-modules-5.0.0-23-generic linux-modules-extra-5.0.0-23-generic


これを削除するには 'sudo apt autoremove' を利用してください。


アップグレード: 0 個、新規インストール: 0 個、削除: 0 個、保留: 435 個。


make -C PCDet


make[1]: ディレクトリ '/home/atusi/CARLA_INVS/PCDet' に入ります


Please ensure you have CUDA 9.0+ and cuDNN installed! (press ENTER to continue...)  ###←？？？？

sudo apt install libboost-all-dev -y


パッケージリストを読み込んでいます... 完了


依存関係ツリーを作成しています                


状態情報を読み取っています... 完了


libboost-all-dev はすでに最新バージョン (1.65.1.0ubuntu1) です。


以下のパッケージが自動でインストールされましたが、もう必要とされていません:


  liblldb-10 libnvidia-common-465 linux-image-5.0.0-23-generic linux-modules-5.0.0-23-generic linux-modules-extra-5.0.0-23-generic


これを削除するには 'sudo apt autoremove' を利用してください。


アップグレード: 0 個、新規インストール: 0 個、削除: 0 個、保留: 435 個。


 sudo apt install llvm-10.0-dev -y; sudo ln -s /usr/bin/llvm-config-10 /usr/bin/llvm-config


pip3 install cmake --user --upgrade


Requirement already satisfied: cmake in /home/atusi/.local/lib/python3.7/site-packages (3.20.2)


pip3 install "torch>=1.1,<=1.4" --user


Requirement already satisfied: torch<=1.4,>=1.1 in /home/atusi/anaconda3/envs/invs/lib/python3.7/site-packages (1.4.0)


rm -rf build/spconv && git clone https://github.com/traveller59/spconv.git build/spconv --recursive


Cloning into 'build/spconv'...
remote: Enumerating objects: 1041, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 1041 (delta 0), reused 2 (delta 0), pack-reused 1036
Receiving objects: 100% (1041/1041), 2.16 MiB | 9.14 MiB/s, done.
Resolving deltas: 100% (701/701), done.
Submodule 'third_party/cutlass' (https://github.com/NVIDIA/cutlass) registered for path 'third_party/cutlass'
Submodule 'third_party/mp11' (https://github.com/boostorg/mp11) registered for path 'third_party/mp11'
Submodule 'third_party/pybind11' (https://github.com/pybind/pybind11.git) registered for path 'third_party/pybind11'
Cloning into '/home/atusi/CARLA_INVS/PCDet/build/spconv/third_party/cutlass'...
remote: Enumerating objects: 11968, done.        
remote: Counting objects: 100% (198/198), done.        
remote: Compressing objects: 100% (134/134), done.        
remote: Total 11968 (delta 83), reused 99 (delta 41), pack-reused 11770        
Receiving objects: 100% (11968/11968), 16.43 MiB | 8.13 MiB/s, done.
Resolving deltas: 100% (8575/8575), done.
Cloning into '/home/atusi/CARLA_INVS/PCDet/build/spconv/third_party/mp11'...
remote: Enumerating objects: 4226, done.        
remote: Counting objects: 100% (177/177), done.        
remote: Compressing objects: 100% (103/103), done.        
remote: Total 4226 (delta 95), reused 132 (delta 59), pack-reused 4049        
Receiving objects: 100% (4226/4226), 813.60 KiB | 1.72 MiB/s, done.
Resolving deltas: 100% (2758/2758), done.
Cloning into '/home/atusi/CARLA_INVS/PCDet/build/spconv/third_party/pybind11'...
remote: Enumerating objects: 15075, done.        
remote: Counting objects: 100% (412/412), done.        
remote: Compressing objects: 100% (220/220), done.        
remote: Total 15075 (delta 229), reused 304 (delta 170), pack-reused 14663        
Receiving objects: 100% (15075/15075), 6.26 MiB | 7.30 MiB/s, done.
Resolving deltas: 100% (10161/10161), done.
Submodule path 'third_party/cutlass': checked out 'c2b80ad4e4f8b60a65500bd04c8fecddff2ba355'
Submodule path 'third_party/mp11': checked out '29764aad4881fde809af6a025c12012e47a55515'
Submodule path 'third_party/pybind11': checked out '3b1dbebabc801c9cf6f0953a4c20b904d444f879'
Submodule 'tools/clang' (https://github.com/wjakob/clang-cindex-python3) registered for path 'third_party/pybind11/tools/clang'
Cloning into '/home/atusi/CARLA_INVS/PCDet/build/spconv/third_party/pybind11/tools/clang'...
remote: Enumerating objects: 368, done.        
remote: Counting objects: 100% (13/13), done.        
remote: Compressing objects: 100% (12/12), done.        
remote: Total 368 (delta 3), reused 6 (delta 1), pack-reused 355        
Receiving objects: 100% (368/368), 159.34 KiB | 11.38 MiB/s, done.
Resolving deltas: 100% (154/154), done.
Submodule path 'third_party/pybind11/tools/clang': checked out '6a00cbc4a9b8e68b71caf7f774b3f9c753ae84d5'
cd build/spconv; python3 setup.py bdist_wheel; cd ./dist; pip3 install *.whl --user
running bdist_wheel
running build
running build_py
creating build
creating build/lib.linux-x86_64-3.7
creating build/lib.linux-x86_64-3.7/spconv
copying spconv/modules.py -> build/lib.linux-x86_64-3.7/spconv
copying spconv/functional.py -> build/lib.linux-x86_64-3.7/spconv
copying spconv/test_utils.py -> build/lib.linux-x86_64-3.7/spconv
copying spconv/identity.py -> build/lib.linux-x86_64-3.7/spconv
copying spconv/conv.py -> build/lib.linux-x86_64-3.7/spconv
copying spconv/pool.py -> build/lib.linux-x86_64-3.7/spconv
copying spconv/tables.py -> build/lib.linux-x86_64-3.7/spconv
copying spconv/ops.py -> build/lib.linux-x86_64-3.7/spconv
copying spconv/__init__.py -> build/lib.linux-x86_64-3.7/spconv
creating build/lib.linux-x86_64-3.7/spconv/utils
copying spconv/utils/__init__.py -> build/lib.linux-x86_64-3.7/spconv/utils
running build_ext
Release
|||||CMAKE ARGS||||| ['-DCMAKE_PREFIX_PATH=/home/atusi/anaconda3/envs/invs/lib/python3.7/site-packages/torch', '-DPYBIND11_PYTHON_VERSION=3.7', '-DSPCONV_BuildTests=OFF', '-DPYTORCH_VERSION=10400', '-DCMAKE_CUDA_FLAGS="--expt-relaxed-constexpr" -D__CUDA_NO_HALF_OPERATORS__ -D__CUDA_NO_HALF_CONVERSIONS__', '-DCMAKE_LIBRARY_OUTPUT_DIRECTORY=/home/atusi/CARLA_INVS/PCDet/build/spconv/build/lib.linux-x86_64-3.7/spconv', '-DCMAKE_BUILD_TYPE=Release']
-- The CXX compiler identification is GNU 7.5.0
-- The CUDA compiler identification is NVIDIA 10.1.243
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Detecting CUDA compiler ABI info
-- Detecting CUDA compiler ABI info - done
-- Check for working CUDA compiler: /usr/local/cuda-10.1/bin/nvcc - skipped
-- Detecting CUDA compile features
-- Detecting CUDA compile features - done
-- Looking for C++ include pthread.h
-- Looking for C++ include pthread.h - found
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Failed
-- Looking for pthread_create in pthreads
-- Looking for pthread_create in pthreads - not found
-- Looking for pthread_create in pthread
-- Looking for pthread_create in pthread - found
-- Found Threads: TRUE  
-- Found CUDA: /usr/local/cuda-10.1 (found version "10.1") 
-- Caffe2: CUDA detected: 10.1
-- Caffe2: CUDA nvcc is: /usr/local/cuda-10.1/bin/nvcc
-- Caffe2: CUDA toolkit directory: /usr/local/cuda-10.1
-- Caffe2: Header version is: 10.1
-- Found CUDNN: /usr/lib/x86_64-linux-gnu/libcudnn.so  
-- Found cuDNN: v?  (include: /usr/include, library: /usr/lib/x86_64-linux-gnu/libcudnn.so)


CMake Error at /home/atusi/anaconda3/envs/invs/lib/python3.7/site-packages/torch/share/cmake/Caffe2/public/cuda.cmake:172 (message):


  PyTorch requires cuDNN 7 and above.
Call Stack (most recent call first):


  /home/atusi/anaconda3/envs/invs/lib/python3.7/site-packages/torch/share/cmake/Caffe2/Caffe2Config.cmake:88 (include)


  /home/atusi/anaconda3/envs/invs/lib/python3.7/site-packages/torch/share/cmake/Torch/TorchConfig.cmake:40 (find_package)


  CMakeLists.txt:22 (find_package)


-- Configuring incomplete, errors occurred!


See also "/home/atusi/CARLA_INVS/PCDet/build/spconv/build/temp.linux-x86_64-3.7/CMakeFiles/CMakeOutput.log".


See also "/home/atusi/CARLA_INVS/PCDet/build/spconv/build/temp.linux-x86_64-3.7/CMakeFiles/CMakeError.log".


Traceback (most recent call last):
  File "setup.py", line 108, in <module>
    zip_safe=False,
  File "/home/atusi/.local/lib/python3.7/site-packages/setuptools/__init__.py", line 153, in setup
    return distutils.core.setup(**attrs)
  File "/home/atusi/anaconda3/envs/invs/lib/python3.7/distutils/core.py", line 148, in setup
    dist.run_commands()
  File "/home/atusi/anaconda3/envs/invs/lib/python3.7/distutils/dist.py", line 966, in run_commands
    self.run_command(cmd)
  File "/home/atusi/anaconda3/envs/invs/lib/python3.7/distutils/dist.py", line 985, in run_command
    cmd_obj.run()
  File "/home/atusi/anaconda3/envs/invs/lib/python3.7/site-packages/wheel/bdist_wheel.py", line 299, in run
    self.run_command('build')
  File "/home/atusi/anaconda3/envs/invs/lib/python3.7/distutils/cmd.py", line 313, in run_command
    self.distribution.run_command(command)
  File "/home/atusi/anaconda3/envs/invs/lib/python3.7/distutils/dist.py", line 985, in run_command
    cmd_obj.run()
  File "/home/atusi/anaconda3/envs/invs/lib/python3.7/distutils/command/build.py", line 135, in run
    self.run_command(cmd_name)
  File "/home/atusi/anaconda3/envs/invs/lib/python3.7/distutils/cmd.py", line 313, in run_command
    self.distribution.run_command(command)
  File "/home/atusi/anaconda3/envs/invs/lib/python3.7/distutils/dist.py", line 985, in run_command
    cmd_obj.run()
  File "setup.py", line 48, in run
    self.build_extension(ext)
  File "setup.py", line 91, in build_extension
    subprocess.check_call(['cmake', ext.sourcedir] + cmake_args, cwd=self.build_temp, env=env)
  File "/home/atusi/anaconda3/envs/invs/lib/python3.7/subprocess.py", line 363, in check_call
    raise CalledProcessError(retcode, cmd)


subprocess.CalledProcessError: Command '['cmake', '/home/atusi/CARLA_INVS/PCDet/build/spconv', 

'-DCMAKE_PREFIX_PATH=/home/atusi/anaconda3/envs/invs/lib/python3.7/site-packages/torch', 

'-DPYBIND11_PYTHON_VERSION=3.7', '-DSPCONV_BuildTests=OFF', '-DPYTORCH_VERSION=10400', 

'-DCMAKE_CUDA_FLAGS="--expt-relaxed-constexpr" -D__CUDA_NO_HALF_OPERATORS__ -D__CUDA_NO_HALF_CONVERSIONS__', 

'-DCMAKE_LIBRARY_OUTPUT_DIRECTORY=/home/atusi/CARLA_INVS/PCDet/build/spconv/build/lib.linux-x86_64-3.7/spconv', 

'-DCMAKE_BUILD_TYPE=Release']' returned non-zero exit status 1.


/bin/bash: 0 行: cd: ./dist: そのようなファイルやディレクトリはありません


WARNING: Requirement '*.whl' looks like a filename, but the file does not exist


ERROR: *.whl is not a valid wheel filename.


Makefile:18: recipe for target 'install-spconv' failed


make[1]: *** [install-spconv] Error 1


make[1]: ディレクトリ '/home/atusi/CARLA_INVS/PCDet' から出ます


Makefile:4: recipe for target 'dependency' failed


make: *** [dependency] Error 2
