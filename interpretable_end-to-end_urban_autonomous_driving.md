# interpretable End-to-end Urban Autonomous Driving with Latent Deep Reinforcement Learning note
## system environment
Carla ver 0.96  
UBUNTU 18.04 LTS  


## requirement
### ffmpeg
sudo apt update  
sudo apt install ffmpeg  
ffmpeg -version  

### cuda10-0
(すでに入っているが今回はcuda10-0が必要なため)  
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-repo-ubuntu1804_10.0.130-1_amd64.deb  
sudo apt install ./cuda-repo-ubuntu1804_10.0.130-1_amd64.deb  
sudo apt update  
sudo apt install --no-install-recommends cuda-10-0  
(/usr/local/にcuda10.0が入っているか確認)  
echo 'export CUDA_PATH=/usr/local/cuda-10.0' >> ${HOME}/.bashrc  
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64:${LD_LIBRARY_PATH}' >> ${HOME}/.bashrc  
echo 'export PATH=/usr/local/cuda-10.0/bin:${PATH}' >> ${HOME}/.bashrc  
reboot  

### Anaconda
refer to (https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart)  

Anacondaをダウンロード(https://www.anaconda.com/distribution/)  
sha256sum Anaconda3-**-Linux-x86_64.sh  
bash Anaconda3-**-Linux-x86_64.sh  
yes  
Enterを押し続ける  
to PATH in your /home/****/.bashrc ? が出てきたら"yes"  
source ~/.bashrc  
conda list  
conda create --name my_env python=3  
conda activate my_env  
ターミナルを消去  

(ターミナルの先頭にbaseが出て続ける場合)  
conda config --set auto_activate_base False  
ターミナルを閉じて新しく開けば改善  


## gym-carla & interp
refer to (https://github.com/cjy1992/interp-e2e-driving)  

conda create -n env_name python=3.6  
conda dactivate env_name  
git clone https://github.com/cjy1992/gym-carla.git  
cd gym-carla  
pip install -r requirements.txt  
pip install -e .  
export PYTHONPATH=${HOME}/(CARLAのフォルダ)/PythonAPI/carla/dist/carla-0.9.6-py3.6-linux-x86_64.egg  

cd ~  
git clone https://github.com/cjy1992/interp-e2e-driving.git  
pip install -r requirements.txt  
pip install -e .  

interp-e2e-driving/interp_e2e_driving/utils/gif_utils.py内の"tf.logging"を"tf.compat.v1.logging"に書き換える(2ヶ所)  

別ターミナルにて./CarlaUE4.sh -carla-port=2000 を実行  
(condaによる仮想環境のターミナルにて)  
cd ~/interp-e2e-driving  
./run_train_eval.sh  
(エラーが出たときはもう一回export PYTHONPATH=${HOME}/(CARLAのフォルダ)/PythonAPI/carla/dist/carla-0.9.6-py3.6-linux-x86_64.eggを試す)  

tensorboard --logdir logs  


## tuning
### out of memory error(これはやらなくていい　要相談)
train_eval.py内のbatch_size(256 → 128)
replay_buffer_capacity(1e5 → 1e1)
initial_collect_steps(1000 → 500)


