# linux-aimax395

This is a Linux 7.1 kernel configuration file prepared for `AMD AI MAX+ 395`. It is specifically configured for local AI large models, with working support for `Vulkan` and `ROCm`.

## Download kernel source

```shell
git clone -b linux-7.1.y --single-branch --depth 1 https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git
```

## Configure build environment

Create `.scmversion` and set local version variables so the build uses the intended kernel version without extra suffixes:

```shell
touch .scmversion
export LOCALVERSION=""
export LOCALVERSION_AUTO=n
```

Copy this repository's configuration file and run `oldconfig` to generate the final `.config` file:

```shell
cp amd_aimax395_defconfig .config
make ARCH=x86_64 oldconfig
```

If you want to review or adjust options manually, run:

```shell
make ARCH=x86_64 menuconfig
```

## Build the kernel packages

Use `bindeb-pkg` to create Debian packages for easy installation:

```shell
make ARCH=x86_64 -j$(nproc) bindeb-pkg
```

The generated `.deb` files are created in the parent directory.

## Install the kernel packages

Install the generated Debian packages:

```shell
sudo dpkg -i ../linux-image-7.1.*.deb
sudo dpkg -i ../linux-headers-7.1.*.deb
sudo dpkg -i ../linux-libc-dev*.deb
```

Reboot after installation and select the new kernel if necessary.

## Firmware update

For the best hardware compatibility, update firmware from the `linux-firmware` repository.

```shell
git clone https://gitlab.com/kernel-firmware/linux-firmware.git
```

Remove conflicting firmware symlinks and old files that may cause install errors:

```shell
sudo find /lib/firmware/ -type l -delete
sudo rm -f /lib/firmware/mediatek/mt8188/scp.img
sudo rm -f /lib/firmware/cirrus/cs35l63-a1-dsp1-misc-10280e1b-*
```

Then enter the firmware directory and install:

```shell
cd linux-firmware
sudo make install
sudo make dedup
```
