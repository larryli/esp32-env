# ESP32 虚拟开发环境

[English](README.md)

## 版本

    - xtensa gcc: 1.22.0-80-g6c4433a-5.2.0
    - esp-idf: v3.2

## 安装 Oracle VM VirtualBox 和 Vagrant

    - [Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Downloads)
    - [Vagrant](https://www.vagrantup.com/downloads.html)

## 开始

    git clone --recursive https://github.com/larryli/esp32-env.git
    cd esp32-env
    vagrant up && vagrant ssh

构建 `hello_world`：

    cp -r $IDF_PATH/examples/get-started/hello_world .
    cd hello_world
    idf.py menuconfig
    idf.py build
    idf.py -p /dev/ttyUSB0 flash
    idf.py -p /dev/ttyUSB0 monitor

更多内容： https://docs.espressif.com/projects/esp-idf/zh_CN/latest/get-started-cmake/index.html#start-a-project

## 配置 USB 串口控制器

运行：

    VBoxManage.exe list usbhost

得到以下输出：

    UUID:               ffade074-82a8-44cd-a730-ebf6072fa666
    VendorId:           0x067b (067B)
    ProductId:          0x2303 (2303)
    Revision:           3.0 (0300)
    Port:               3
    USB version/speed:  1/Full
    Manufacturer:       Prolific Technology Inc.
    Product:            USB-Serial Controller
    Current State:      Available

然后编辑 `vagrant.yml`：

```yaml
usbfilters:
  - name: pl2303
    vendorid: 067b
    productid: "2303"
```

重启虚拟环境：

    vagrant reload

## 手工下载 Ubuntu 镜像（可选）

如果 `vagrant up` 自动下载 Ubuntu 镜像太慢，可以在执行 `vagrant up` 前选择手工下载镜像：

    curl -O http://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64-vagrant.box

下载成功后，增加：

    vagrant box add --name 'ubuntu/bionic64' bionic-server-cloudimg-amd64-vagrant.box

## 使用 Ubuntu APT 源、PIP 镜像（可选）

如果安装软件包太慢，可以在执行 `vagrant up` 前复制 `vagrant.example.yml` 为 `vagrant.yml`：

    copy vagrant.example.yml vagrant.yml

然后编辑 `vagrant.yml`：

```yaml
apt_source: cn.archive.ubuntu.com
pip_simple: https://pypi.doubanio.com/simple/
```
