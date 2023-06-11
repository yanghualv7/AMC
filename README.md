# AMC
# ChatGLM-6B 离线部署



## 一、GitHub地址

https://github.com/THUDM/ChatGLM-6B.git



## 二、本机硬件配置

#### 在终端输入以下命令查看显卡、cuda

```ubuntu命令
输入：
nvidia-smi

# 若未安装cuda请运行以下命令：
# sudo apt install nvidia-cuda-toolkit

输出：
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 470.74       Driver Version: 470.74       CUDA Version: 11.4     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce ...  Off  | 00000000:1A:00.0 Off |                  N/A |
|  0%   27C    P8    19W / 300W |     10MiB / 11019MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   1  NVIDIA GeForce ...  Off  | 00000000:68:00.0  On |                  N/A |
|  0%   41C    P8     8W / 300W |    190MiB / 11016MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A      1264      G   /usr/lib/xorg/Xorg                  4MiB |
|    0   N/A  N/A     27928      G   /usr/lib/xorg/Xorg                  4MiB |
|    1   N/A  N/A      1264      G   /usr/lib/xorg/Xorg                 18MiB |
|    1   N/A  N/A      1544      G   /usr/bin/gnome-shell               70MiB |
|    1   N/A  N/A     27928      G   /usr/lib/xorg/Xorg                 82MiB |
|    1   N/A  N/A     28056      G   /usr/bin/gnome-shell               14MiB |
+-----------------------------------------------------------------------------+

输入：
nvidia-smi -L

输出：
GPU 0: NVIDIA GeForce RTX 2080 Ti (UUID: GPU-8fb0ceb2-75ca-5ef6-126b-a6ec47104fe2)
GPU 1: NVIDIA GeForce RTX 2080 Ti (UUID: GPU-dfc215c4-1ece-27b1-49db-97e6c5e9df3a)

输入：
nvcc -V

输出：
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2021 NVIDIA Corporation
Built on Sun_Mar_21_19:15:46_PDT_2021
Cuda compilation tools, release 11.3, V11.3.58
Build cuda_11.3.r11.3/compiler.29745058_0

#11.3为cuda版本，之后用conda创建虚拟环境时需要指定为该版本
```



## 三、配置步骤

## （一）、anaconda安装

#### 1.创建安装目录

```ubuntu命令
mkdir AMC

cd AMC/

```

#### 2.下载、安装anaconda

```ubuntu命令
wget https://repo.anaconda.com/archive/Anaconda3-2023.03-1-Linux-x86_64.sh

bash Anaconda3-2023.03-1-Linux-x86_64.sh -p anaconda/ -u

echo 'export PATH="/home/zlab1/AMC/anaconda/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

## （二）、安装git工具

Git 软件包被包含在 Ubuntu 默认的软件源仓库中，并且可以使用 apt 包管理工具安装。

#### 1.以 sudo 权限用户身份运行下面的命令更新软件源和安装：

```ubuntu命令
sudo apt update

sudo apt install git
```

#### 2.运行下面的命令，打印 Git 版本，验证安装过程：

```ubuntu命令
git --version
```

## （三）启用ufw

#### 1.启用

```ubuntu命令
sudo ufw enable
```

#### 2.开放端口

```ubuntu命令
sudo ufw allow 8080
```

#### 3.查看

```
sud0 ufw status
```



## （四）、下载源码及模型

#### 1.下载源码

```ubuntu命令
cd /home/zlab1/AMC && ls

mkdir ChatGLM && cd ChatGLM/

git clone https://github.com/yanghualv7/ChatGLM-6B.git

# 若克隆失败可运行下面命令再次尝试
# git config --global http.postBuffer 524288000
```

#### 2.下载模型

```ubuntu命令
mkdir model && cd model/

git lfs install
git clone https://huggingface.co/THUDM/chatglm-6b-int4

```

#### 3.修改模型为本机路径

```ubuntu命令
cd /home/zlab1/AMC/ChatGLM/ChatGLM-6B && ls

vim web_demo2.py # 将框出部分的路径改为下载的模型路径
```

![image-20230610160354984](https://thumbnail0.baidupcs.com/thumbnail/e8d7dc880gf2c539222567469f54b12f?fid=2794798890-250528-982345225605206&time=1686387600&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-jrJCt%2Bmbz2VhTZHVKb%2BwzxeOn1s%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=269054173777089613&dp-callid=0&file_type=0&size=c710_u400&quality=100&vuk=-&ft=video)

```ubuntu命令
vim requirements.txt
```

![image-20230610161357609](https://thumbnail0.baidupcs.com/thumbnail/7f7f78a99s375ad0f549eda7f53c75af?fid=2794798890-250528-634649274020223&time=1686481200&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-SNYmTreESopnFQii0%2F6iofou578%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=293794428961997931&dp-callid=0&file_type=0&size=c710_u400&quality=100&vuk=-&ft=video)

### (五)、conda创建ChatGLM虚拟环境

```ubuntu命令
conda create -n ChatGLM python=3.10 cudatoolkit=11.3

conda activate ChatGL

```



## 四.运行demo

```ubuntu命令
python3 -m streamlit run ./web_demo2.py --server.port 8080

#使用框出部分的链接访问，第二个还不清除为什么不能访问
```

![image-20230610162912849](https://thumbnail0.baidupcs.com/thumbnail/add307a65u11bb6296acdb04dc6f835d?fid=2794798890-250528-1038593971517293&time=1686387600&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-dmsiaLjyP5l%2BA1M3LA8quhHqdVk%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=269073964830030885&dp-callid=0&file_type=0&size=c710_u400&quality=100&vuk=-&ft=video)![image-20230610163124868](https://thumbnail0.baidupcs.com/thumbnail/d26363058h8465889f116e854edeeb52?fid=2794798890-250528-624400145802704&time=1686387600&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-nQ%2BrOKQ%2Fx4NmGIUTmIt8a6r7l64%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=269137708484155471&dp-callid=0&file_type=0&size=c710_u400&quality=100&vuk=-&ft=video)
