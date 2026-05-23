# linux-aimax395

Linux kernel for AMD AI MAX+395

## config

```shell
make amd_aimax395_defconfig
```

## build

```shell
make -j$(nproc) all
```

## install modules

```shell
sudo make modules_install INSTALL_MOD_PATH=/tmp/rootfs
```

## install linux headers

```shell
sudo make headers_install INSTALL_HDR_PATH=/tmp/rootfs/usr
```

## install firmware

```shell
sudo apt install firmware-amd-graphics amd64-microcode lshw firmware-linux-free firmware-linux firmware-linux-nonfree
```
