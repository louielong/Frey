From ubuntu:16.04

MAINTAINER Louie Long <longyu805@163.com>
LABEL version="0.1" description="esp32 build environment based on ubuntu 16.04"

# 使用网易源
RUN  sed -i s@/archive.ubuntu.com/@/mirrors.163.com/@g /etc/apt/sources.list

# 添加颜色显示
RUN echo "alias ls='ls --color=auto'" >> /root/.bashrc

# 使用ubuntu安装必要的软件
# https://docs.espressif.com/projects/esp-idf/zh_CN/latest/get-started/linux-setup.html
RUN \
        apt-get update \
        && \
        apt-get install -y \
        vim \
        git \
        gcc \
        make \
        wget \
        flex \
        bison \
        gperf \
        python \
        net-tools \
        python-pip \
        iputils-ping \
        python-serial \
        python-future \
        libncurses-dev \
        python-pyparsing \
        python-pyelftools \
        python-setuptools \
        python-cryptography \
        --no-install-recommends \
        && rm -rf /var/lib/apt/lists/*

RUN cd /root && \
    wget https://dl.espressif.com/dl/xtensa-esp32-elf-linux64-1.22.0-80-g6c4433a-5.2.0.tar.gz -O xtensa-esp32-elf-linux64.tar.gz && \
    tar xvf xtensa-esp32-elf-linux64.tar.gz -C /usr/share --remove-files && \
    rm xtensa-esp32-elf-linux64.tar.gz && \
    echo 'export PATH="/usr/share/xtensa-esp32-elf/bin:$PATH"'  >> /root/.bashrc && \
    git clone --recursive https://github.com/espressif/esp-idf.git && \
    cd esp-idf && \
    git checkout -b v3.2 && \
    echo 'export IDF_PATH=/root/esp-idf' >> /root/.bashrc && \
    . /root/.bashrc && \
    pip install wheel && \
    python -m pip install --user -r $IDF_PATH/requirements.txt


# 指定工作目录
WORKDIR /root

# 指定家目录文件
ENV HOME /root

# 中文支持
ENV LANG C.UTF-8

#ENTRYPOINT /bin/bash
