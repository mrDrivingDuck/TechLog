# Ubuntu - Installation on Lenovo R720

Created by : Mr Dk.

2019 / 01 / 17 15:45

Nanjing, Jiangsu, China

---

## GPU Problem

系统安装完毕后重启，结果整个界面卡死，应该是显卡冲突的问题。进入系统后，发现 *Ubuntu* 使用了集成显卡，没有使用 *NVIDIA*。

![ubuntu-without-nvidia](../img/ubuntu-without-nvidia.png)

因此需要安装 *NVIDIA* 的附加驱动：

```console
$ ubuntu-drivers devices
```

![ubuntu-drivers-devices](../img/ubuntu-drivers-devices.png)

```console
$ sudo ubuntu-drivers autoinstall
```

之后重启电脑，查看效果：

![ubuntu-nvidia](../img/ubuntu-nvidia.png)

![ubuntu-with-nvidia](../img/ubuntu-with-nvidia.png)

---

## Wireless Interface Problem

在 *Terminal* 中输入：

```console
$ rfkill list all
```

*Lenovo R720* 的无线网卡被硬件关闭了，因此在操作系统中无法通过软件打开。通过列一个黑名单，将硬件关闭屏蔽：

```console
$ sudo touch /etc/modprobe.d/ideapad.conf
$ sudo gedit /etc/modprobe.d/ideapad.conf 
```

```
blacklist ideapad_laptop
```

保存文件并关闭，移除设备：

```console
$ sudo modprobe -r ideapad_laptop 
```

重启电脑。

---

