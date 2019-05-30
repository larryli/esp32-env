# ESP32 Virtual Development Environment

[中文](README-CN.md)

## Version

- xtensa gcc: 1.22.0-80-g6c4433a-5.2.0
- esp-idf: v3.2

## Install Oracle VM VirtualBox and Vagrant

- [Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Vagrant](https://www.vagrantup.com/downloads.html)

## Get Started

    git clone --recursive https://github.com/larryli/esp32-env.git
    cd esp32-env
    vagrant up && vagrant ssh

Build `hello_world`:

    cp -r $IDF_PATH/examples/get-started/hello_world .
    cd hello_world
    idf.py menuconfig
    idf.py build
    idf.py -p /dev/ttyUSB0 flash
    idf.py -p /dev/ttyUSB0 monitor

More: https://docs.espressif.com/projects/esp-idf/en/latest/get-started-cmake/index.html#start-a-project

## Configure USB Serial Controller

Run:

    VBoxManage.exe list usbhost

Output:

    UUID:               ffade074-82a8-44cd-a730-ebf6072fa666
    VendorId:           0x067b (067B)
    ProductId:          0x2303 (2303)
    Revision:           3.0 (0300)
    Port:               3
    USB version/speed:  1/Full
    Manufacturer:       Prolific Technology Inc.
    Product:            USB-Serial Controller
    Current State:      Available

Edit `vagrant.yml`:

```yaml
usbfilters:
  - name: pl2303
    vendorid: 067b
    productid: "2303"
```

Reload virtual machine:

    vagrant reload

## Download Ubuntu Image Manually (Optional)

Download the image before `vagrant up`:

    curl -O http://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64-vagrant.box

Add box:

    vagrant box add --name 'ubuntu/bionic64' bionic-server-cloudimg-amd64-vagrant.box

## Use Ubuntu APT Source, PIP Mirror (Optional)

Copy `vagrant.example.yml` to `vagrant.yml` before `vagrant up`:

    copy vagrant.example.yml vagrant.yml

Edit `vagrant.yml`:

```yaml
apt_source: cn.archive.ubuntu.com
pip_simple: https://pypi.doubanio.com/simple/
```

## FAQ

1. Exec `idf.py -p /dev/ttyUSB0 flash` get error message after first `vagrant up`:

    [Errno 2] could not open port /dev/ttyUSB0: [Errno 2] No such file or directory: '/dev/ttyUSB0'

This is because the `linux-image-extra-virtual` package required to restart the system that supports the USB serial controller. Please run:

    vagrant reload
