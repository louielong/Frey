# ESP32  Build Enviroment Using Docker 

## 容器构建

docker容器构建参考：[ESP-IDF编程](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/get-started/index.html#)

### 安装必要软件包

以ubuntu16.04-64bit为基础系统，安装必要的软件包

```shell
sudo apt-get install gcc git wget make libncurses-dev flex bison gperf python python-pip python-setuptools python-serial python-cryptography python-future python-pyparsing python-pyelftools
```

下载交叉编译工具：

```shell
cd /root
wget https://dl.espressif.com/dl/xtensa-esp32-elf-linux64-1.22.0-80-g6c4433a-5.2.0.tar.gz -O xtensa-esp32-elf-linux64.tar.gz
tar xvf xtensa-esp32-elf-linux64.tar.gz -C /usr/share
```

下载ESP-IDF源码：

```shell
git clone --recursive https://github.com/espressif/esp-idf.git
cd esp-idf
git checkout -b v3.2   # 使用3.2版本
```

导入环境变量：

```shell
echo 'export PATH="/usr/share/xtensa-esp32-elf/bin:$PATH"'  >> /root/.bashrc
echo 'export IDF_PATH=/root/esp-idf' >> /root/.bashrc
```

安装python软件包：

```shell
source /root/.bashr
pip install wheel
python -m pip install --user -r $IDF_PATH/requirements.txt
```