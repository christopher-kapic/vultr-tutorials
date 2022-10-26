## Using Rife for Frame Interpolation on Vultr's GPU VMs

0. Provision the VM (Ubuntu 20.04)

1. Install ffmpeg and Anaconda

```bash
cd ~/

sudo wget -O /etc/apt/preferences.d/cuda-repository-pin-600 https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/7fa2af80.pub
sudo add-apt-repository "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/ /"

wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb

sudo apt update
sudo apt upgrade

apt install -y ffmpeg cuda

wget https://repo.anaconda.com/archive/Anaconda3-latest-Linux-x86_64.sh -O ~/anaconda.sh
bash ~/anaconda.sh -b -p $HOME/anaconda

git clone https://github.com/hzwer/Practical-RIFE.git

exit
```

2. Install requirements

```bash
# new remote shell
cd ~/Practical-RIFE
pip3 install -r requirements.txt
pip3 install torchvision opencv-python moviepy
conda install -c conda-forge scikit-video
```

3. Upload video and model (model can be downloaded [here](https://drive.google.com/file/d/1EAbsfY7mjnXNa6RAsATj2ImAEqmHTjbE/view)) using SCP (on local machine)

```bash
# local shell
scp RIFE_trained_model_v4.6.zip root@ip:~/Practical-RIFE/RIFE_trained_model_v4.6.zip
scp video.mp4 root@ip:~/video.mp4
```

4. Unzip training file

```bash
# remote shell
unzip RIFE_trained_model_v4.6.zip
```

5. Optimize the video

```bash
# remote shell
python3 inference_video.py --multi=2 --video=~/video.mp4
```

6. Download output video with SCP

```bash
# local shell
scp root@ip:~/video_output.mp4 \\ # name depends on original video and how much you interpolated
  ~/video_output.mp4
```
