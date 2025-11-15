---
title: Android 16 原生终端体验
published: 2025-11-15
description: "总结：夯"
image: ./SS1.jpg
tags: ["Blogging", "Android", "Linux"]
category: Experience
draft: false
---

> 昨天在鼓捣我的备用机，想要运行一个完备的Linux环境时，偶然在小红薯上看到了HyperOS 3的Linux终端功能，刚刚好我的设备是MTK，支持这个功能:spoiler[~~Qualcomm还不支持哩！~~]，果真列在我的开发者选项中。
> 
> 据说支持GPU加速，可以跑桌面环境。

# 支持GPU加速！

如图，`/dev/dri`目录下有东西！！

## 运行`systemctl`

我尝试运行`systemctl`，成功了！

```bash title="ssh" collapse={16-54}
~ $ ssh xiaozhao45@0.0.0.0 -p 2222
xiaozhao45@0.0.0.0's password: 
Linux localhost 6.1.0-34-avf-arm64 #1 SMP Debian 6.1.135-1 (2025-04-25) aarch64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sat Nov 15 06:40:51 2025 from 127.0.0.1
xiaozhao45@localhost:~$ systemctl
  UNIT                                                                                              LOAD   ACTIVE SUB       DESCRIP>
  proc-sys-fs-binfmt_misc.automount                                                                 loaded active running   Arbitra>
  sys-devices-platform-2e000000.pci-pci0000:00-0000:00:02.0-virtio1-virtio\x2dports-vport1p0.device loaded active plugged   /sys/de>
  sys-devices-platform-2e000000.pci-pci0000:00-0000:00:03.0-virtio2-virtio\x2dports-vport2p0.device loaded active plugged   /sys/de>
  sys-devices-platform-2e000000.pci-pci0000:00-0000:00:04.0-virtio3-virtio\x2dports-vport3p0.device loaded active plugged   /sys/de>
  sys-devices-platform-2e000000.pci-pci0000:00-0000:00:05.0-virtio4-block-vda-vda1.device           loaded active plugged   /sys/de>
  sys-devices-platform-2e000000.pci-pci0000:00-0000:00:05.0-virtio4-block-vda-vda2.device           loaded active plugged   /sys/de>
  sys-devices-platform-2e000000.pci-pci0000:00-0000:00:05.0-virtio4-block-vda.device                loaded active plugged   /sys/de>
  sys-devices-platform-2e000000.pci-pci0000:00-0000:00:07.0-virtio6-net-enp0s7.device               loaded active plugged   Virtio >
  sys-devices-platform-2e8.U6_16550A-tty-ttyS3.device                                               loaded active plugged   /sys/de>
  sys-devices-platform-2f8.U6_16550A-tty-ttyS1.device                                               loaded active plugged   /sys/de>
  sys-devices-platform-3e8.U6_16550A-tty-ttyS2.device                                               loaded active plugged   /sys/de>
  sys-devices-platform-3f8.U6_16550A-tty-ttyS0.device                                               loaded active plugged   /sys/de>
  sys-devices-virtual-block-zram0.device                                                            loaded active plugged   /sys/de>
  sys-devices-virtual-tty-hvc0.device                                                               loaded active plugged   /sys/de>
  sys-devices-virtual-tty-hvc1.device                                                               loaded active plugged   /sys/de>
  sys-devices-virtual-tty-hvc2.device                                                               loaded active plugged   /sys/de>
  sys-devices-virtual-tty-hvc3.device                                                               loaded active plugged   /sys/de>
  sys-devices-virtual-tty-hvc4.device                                                               loaded active plugged   /sys/de>
  sys-devices-virtual-tty-hvc5.device                                                               loaded active plugged   /sys/de>
  sys-devices-virtual-tty-hvc6.device                                                               loaded active plugged   /sys/de>
  sys-devices-virtual-tty-hvc7.device                                                               loaded active plugged   /sys/de>
  sys-module-configfs.device                                                                        loaded active plugged   /sys/mo>
  sys-module-fuse.device                                                                            loaded active plugged   /sys/mo>
  sys-subsystem-net-devices-enp0s7.device                                                           loaded active plugged   Virtio >
  -.mount                                                                                           loaded active mounted   Root Mo>
  dev-hugepages.mount                                                                               loaded active mounted   Huge Pa>
  dev-mqueue.mount                                                                                  loaded active mounted   POSIX M>
  mnt-internal.mount                                                                                loaded active mounted   /mnt/in>
  mnt-shared.mount                                                                                  loaded active mounted   /mnt/sh>
  opt-kernel_extras.mount                                                                           loaded active mounted   /opt/ke>
  proc-sys-fs-binfmt_misc.mount                                                                     loaded active mounted   Arbitra>
  run-credentials-systemd\x2dsysctl.service.mount                                                   loaded active mounted   /run/cr>
  run-credentials-systemd\x2dsysusers.service.mount                                                 loaded active mounted   /run/cr>
  run-credentials-systemd\x2dtmpfiles\x2dsetup.service.mount                                        loaded active mounted   /run/cr>
  run-credentials-systemd\x2dtmpfiles\x2dsetup\x2ddev.service.mount                                 loaded active mounted   /run/cr>
  run-user-0.mount                                                                                  loaded active mounted   /run/us>
  run-user-1000.mount                                                                               loaded active mounted   /run/us>
  run-user-1001.mount                                                                               loaded active mounted   /run/us>
  sys-fs-fuse-connections.mount                                                                     loaded active mounted   FUSE Co>
  sys-kernel-config.mount                                                                           loaded active mounted   Kernel >
  sys-kernel-debug-tracing.mount                                                                    loaded active mounted   /sys/ke>
xiaozhao45@localhost:~$ 

```

## 跑Podman

我尝试跑了一下Podman。可以直接用`apt`安装。

:::note[Podman是什么？]
Podman 是一款由 Red Hat 开发的开源、无守护进程的 Linux 容器引擎，用于创建、运行和管理 OCI 兼容的容器和容器镜像。它作为 Docker 的直接替代品，提供了与 Docker 命令高度兼容的 CLI，但其采用无守护进程架构且默认以非 root 用户运行，从而提升了安全性和灵活性。
:::

:::tip[总结]
类似Docker，在同时提供Podman和Docker的发行版中，能正常运行Podman就意味着Docker也应该能正常工作。
:::



```bash title="ssh"
xiaozhao45@localhost:~$ podman exec -it suse /bin/bash
root@51b9e53a3177:/# neofetch
         JJJJJJJJ                            root@51b9e53a3177 
      JJJJJJJJJJJJJJ                         ----------------- 
    JJJJJJ   =JJJJJJJ                        OS: openSUSE Tumbleweed aarch64 
   JJJJ      =JJJ JJJJ                       Kernel: 6.1.0-34-avf-arm64 
   JJJ       =JJJ   JJJ                      Uptime: 35 mins 
  JJJJ       =JJJ   JJJ                      Packages: 241 (rpm) 
  JJJJJJJJJJJJJJJ   JJJJ                     Shell: bash 5.3.3 
   JJJJJJJJJJJJJJ   JJJJ                     Resolution: 1280x1024 
   JJJJ             JJJJ                     Terminal: gtk-cursor-theme-name 
    JJJJJ=          JJJJ                     CPU: ARM Cortex-A725 (8) 
      JJJJJJJJJJJJJJJJJJJJJJJJJJJJJ=         Memory: 0.44 GiB / 3.83 GiB (11%) 
        =JJJJJJJJJJJJJJJJJJJJJJJJJJJJJ
                    JJJJ         =JJJJJJ                             
                    JJJJ            =JJJJ                            
                    JJJJ   JJJJJJJJJJJJJJ
                    JJJJ   JJJJJJJJJJJJJJJ
                    JJJJ   JJJJ       JJJJ
                     JJJ   JJJJ       JJJ
                     JJJJJ JJJJ      JJJJ
                      =JJJJJJJJ   JJJJJJ
                        JJJJJJJJJJJJJJ
                           JJJJJJJ=

root@51b9e53a3177:/# 
```

这意味着我们终于不再拘泥于PRoot、Chroot这类不完整或是需要root权限的容器了，不过...

# 缺点

## 支持的SoC少

只支持MTK等SoC，甚至新的骁龙 8 Elite Gen 5 也不支持。

## 磁盘空间分配不灵活

这个很重要，这就像你只能创建一个立即分配空间的虚拟机。

这意味着在里面调整磁盘大小时，这个终端程序会直接“吃掉”这部分。

<div style="display:flex;gap:20px">

<img src="https://github.com/xiaozhao45/Images/blob/main/SS2.jpg?raw=true" style="height:400px">

<img src="https://github.com/xiaozhao45/Images/blob/main/SS3.jpg?raw=true" style="height:400px">

</div>

*<center>一秒就装了个原神（bushi）。</center>*

## ssh不支持外部连接

在我的使用中，我可以用Termux运行`ssh username@0.0.0.0 -p <port>`来连接这个系统，但是我在我的电脑、备用机上无论如何也无法连接，设置中的端口控制已经放行了2222，也就是我的自定义ssh端口。最终还是用termux开ssh，再连接。总之麻烦极了。

# 总之

总之，我看了看我的备用机，即使也是MTK、也是Android 16，但不支持这项功能，而我的主力机再容不下这个不能动态磁盘空间占用的系统。

我先前摆弄过好一阵的Termux和内核编译，目的也是跑个完整的Linux。从PRoot、Chroot到编译内核，最完备的就是本文所讲的终端，不过有些瑕疵罢了，我也不会在我的主力机上给它分配多少空间。

:::note
考虑过使用NFS连到我的备用机或者云盘，再在云上跑Podman，不过这有些鸡肋了，断网就废。
:::

目前来看，这个功能更多是用来跑一个桌面环境、扩展生态，这对没有电脑的人很有帮助，未来也可能会像WSL一样发展，还是很未来可期的。