## 使用conda安装依赖
```shell
conda install --yes --file requirements.txt
```

## conda 换源
```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge 
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
# 设置搜索时显示通道地址 从channel中安装包时显示channel的url，这样就可以知道包的安装来源
conda config --set show_channel_urls yes
```

## ImportError: libGL.so.1
```shell
apt install libgl1-mesa-glx
```

## apt-get update cuda源 public key is not available
```shell
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub
```

## build: ImportError: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.29' not found (required by /usr/local/lib/python3.8/site-packages/rknn/api/lib/linux-x86_64/cp38/librknnc.so)
```shell
wget https://ftp.gnu.org/gnu/glibc/glibc-2.28.tar.gz
$ tar -xzvf glibc-2.28.tar.gz
$ cd glibc-2.28

# 创建临时文件
$ mkdir build && cd build

# 配置环境 
$ ../configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin

在配置环境时，大概率会报编译工具过旧 These critical programs are missing or too old: make compiler，需要升级gcc和make。

apt-get install software-properties-common 
add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get update
sudo apt-get install gcc-10
sudo apt-get install g++-10
cd /usr/bin
sudo rm gcc g++
sudo ln -s gcc-10 gcc
sudo ln -s g++-10 g++

# 安装（此步可能会导致系统错误，建议先看完本文再执行）
$ make
$ make install

# 查询安装结果
$ strings /lib64/libc.so.6 | grep GLIBC