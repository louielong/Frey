# ESP32  Build Enviroment Using Docker 



## 容器使用

降低windows平台上环境搭建的复杂度，使用docker构建了一个编译环境

### 下载容器镜像：

```shell
docker pull l0uie/frey
```

### 启动容器

```shell
docker run -itd -v <workplace>:/root/workplace  --device=/dev/ttyS2:/dev/ttyS2 --name esp32 l0uie/frey
```

其中：

- `-v <workplace>:/root/workplace`  映射宿主机项目代码
- `--device=/dev/ttyS2:/dev/ttyS2` 映射宿主机tty终端，视具体的tty而定

### 代码编译

以*ESP-IDF*自带的*hello_world*编译为例，**进入容器后**

```shell
cd /root
cp -r $IDF_PATH/examples/get-started/hello_world .
cd hello_world/
vi Makefile
```

修改Makefile ，将 *include $(IDF_PATH)/make/project.mk*改为

> include ${IDF_PATH}/make/project.mk

### 编译选项配置

配置编译选项

```shell
make menuconfig
```

![编译选项配置](https://docs.espressif.com/projects/esp-idf/zh_CN/latest/_images/project-configuration1.png)

![编译配置](https://raw.githubusercontent.com/louielong/blogPic/master/img20190530164456.png)

其中：

1）修改下载串口为对应的串口设备

2）串口波特率选择**115200**

3）SPI下载选择**DIO**

4）晶振选择**40MHz**

【Note】

也可以跳过MenuConfig配置直接设置相应参数

用户可以在使用 `make` 命令时 **直接设置** 部分环境变量，而无需进入 `make menuconfig` 进行重新配置。这些变量包括：

| 变量                                                         | 描述与使用方式                             |
| ------------------------------------------------------------ | ------------------------------------------ |
| `ESPPORT`                                                    | 覆盖 `flash` 和 `monitor` 命令使用的串口。 |
| 例：`make flash ESPPORT=/dev/ttyUSB1`, `make monitor ESPPORT=COM1` |                                            |
| `ESPBAUD`                                                    | 覆盖烧录 ESP32 时使用的串口速率。          |
| 例：`make flash ESPBAUD=9600`                                |                                            |
| `MONITORBAUD`                                                | 覆盖监控时使用的串口速率。                 |
| 例：`make monitor MONITORBAUD=9600`                          |                                            |

### 编译并烧录bin文件

若想直接烧录编译后的文件(**确保串口已经映射**)，执行

```shell
make flash
```

### 只编译*hello_world*

```shell
make all
......
LD build/hello-world.elf
esptool.py v2.7-dev
To flash all build output, run 'make flash' or:
python /root/esp-idf/components/esptool_py/esptool/esptool.py --chip esp32 --port /dev/ttyUSB0 --baud 115200 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 40m --flash_size detect 0x1000 /root/hello_world/build/bootloader/bootloader.bin 0x10000 /root/hello_world/build/hello-world.bin 0x8000 /root/hello_world/build/partitions_singleapp.bin
```

随后可以通过拷贝bin文件和其他下载工具进行烧录



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
git checkout -b release/v3.2   # 使用v3.2版本
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



