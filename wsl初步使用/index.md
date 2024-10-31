# Wsl初步使用



## 安装
1.选择发行版本并安装
```
 wsl --list --online
以下是可安装的有效分发的列表。
使用 'wsl.exe --install <Distro>' 安装。

NAME                            FRIENDLY NAME
Ubuntu                          Ubuntu
Debian                          Debian GNU/Linux
kali-linux                      Kali Linux Rolling
Ubuntu-18.04                    Ubuntu 18.04 LTS
Ubuntu-20.04                    Ubuntu 20.04 LTS
Ubuntu-22.04                    Ubuntu 22.04 LTS
Ubuntu-24.04                    Ubuntu 24.04 LTS
OracleLinux_7_9                 Oracle Linux 7.9
OracleLinux_8_7                 Oracle Linux 8.7
OracleLinux_9_1                 Oracle Linux 9.1
openSUSE-Leap-15.6              openSUSE Leap 15.6
SUSE-Linux-Enterprise-15-SP5    SUSE Linux Enterprise 15 SP5
SUSE-Linux-Enterprise-15-SP6    SUSE Linux Enterprise 15 SP6
openSUSE-Tumbleweed             openSUSE Tumbleweed

```
比如我选择：Ubuntu-24.04
```
wsl --install -d Ubuntu-24.04
```
等待安装完成
2.重启
3.设置账户和密码
4.查看wsl安装列表
```
 wsl --list --verbose
  NAME            STATE           VERSION
* Ubuntu-24.04    Running         2
```
可以看到Ubuntu-24.04已经安装运行，并且是使用wsl 2

## Ubuntu挂载U盘(SD卡)

1. 在/mnt 目录下新建想要挂载的盘目录，比如：
```
sudo mkdir /mnt/d
```

2. 挂载Windows的文件系统到Linux的挂载点,例如：
```
sudo mount -t drvfs D:\\ /mnt/d

```

3. 卸载挂载点
```
sudo umount /mnt/d
```


