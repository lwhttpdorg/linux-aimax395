# linux-aimax395

This is a kernel(version 7.1) maintained for the `AMD AI MAX+ 395`. `Vulkan` and `ROCm` are working properly.

_Note: I did not modify any kernel source code. In fact, I just meticulously configured the `amd_aimax395_defconfig` for
running local LLMs (Large Language Models) on the AMD AI MAX+ 395. You can use this configuration file to compile directly from the official Linux source code._

## 1. Compiling the Kernel

### 1.1. config

```shell
touch .scmversion
export LOCALVERSION=""
export LOCALVERSION_AUTO=n
```

```shell
make ARCH=x86_64 amd_aimax395_defconfig
```

### 1.2. build

```shell
make ARCH=x86_64 -j$(nproc) bindeb-pkg
```

### 1.3. install

```shell
sudo dpkg -i ../linux-image-7.1.*.deb
sudo dpkg -i ../linux-headers-7.1.*.deb
sudo dpkg -i ../linux-libc-dev*.deb
```

## 2. Updating Firmware

Clone the `linux-firmware` repository

```shell
git clone https://gitlab.com/kernel-firmware/linux-firmware.git
```

Clean up conflicting firmware links and files to avoid errors during `make install`

```shell
sudo find /lib/firmware/ -type l -delete
sudo rm -f /lib/firmware/mediatek/mt8188/scp.img
sudo rm -f /lib/firmware/cirrus/cs35l63-a1-dsp1-misc-10280e1b-*
```

Enter the directory, compile, and install:

```shell
cd linux-firmware
sudo make install
```

```shell
sudo make dedup
```
