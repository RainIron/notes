# 安装Archlinux系统

- 引导方式使用UEFI
- 镜像：archlinux-2021.04.01-x86_64.iso

- 环境vmware



1.  验证启动模式

   ```
   # ls /sys/firmware/efi/efivars
   ```

   > 如果命令显示的目录没有错误，则系统以UEFI模式启动。如果该目录不存在，则可以在BIOS模式下引导系统。

2.  连接到互联网
   - 有线连接会自动分配ip
   - wifi连接使用iwctl对无线网络进行身份验证
   - 移动宽带调制解调器使用mmcli实用程序连接到移动网络

3.  更新时钟

   ```
   timedatectl set-ntp true
   ```

4.   分区磁盘

   查看分区状态

   ```
   # fdisk -l
   ```

    进行分区

   ```
   # cfdisk /dev/sda
   ```

   分区布局

   ​                                                                                                                         <b>BIOS分区布局</b>

   |  挂载点  |          划分           | [分区类型](https://en.wikipedia.org/wiki/Partition_type) |    建议尺寸    |
   | :------: | :---------------------: | :------------------------------------------------------: | :------------: |
   | `[SWAP]` | `/dev/*swap_partition*` |                        Linux交换                         |  超过512 MiB   |
   |  `/mnt`  | `/dev/*root_partition*` |                         的Linux                          | 设备的其余部分 |

   ​                                                                                                                       <b>UEFI分区布局</b>

   |           挂载点            |             划分              | [分区类型](https://en.wikipedia.org/wiki/GUID_Partition_Table#Partition_type_GUIDs) |    建议尺寸    |
   | :-------------------------: | :---------------------------: | :----------------------------------------------------------: | :------------: |
   | `/mnt/boot` 或者 `/mnt/efi` | `/dev/*efi_system_partition*` | [EFI系统分区](https://wiki.archlinux.org/index.php/EFI_system_partition) |  至少260 MiB   |
   |          `[SWAP]`           |    `/dev/*swap_partition*`    |                          Linux交换                           |  超过512 MiB   |
   |           `/mnt`            |    `/dev/*root_partition*`    |                     Linux x86-64根（/）                      | 设备的其余部分 |

5.  格式化分区

   格式化引导分区

   ```
   # mkfs.fat -F32 /dev/sda1
   ```

   格式化主分区

   ```
   # mkfs.ext4 /dev/sda3
   ```

   格式化swap分区

   ```
   # mkswap /dev/sda2
   ```

6.  挂载文件系统

   将主分区挂载到/mnt

   ```
   # mount /dev/sda3 /mnt
   ```

   挂载引导分区

   ```
   创建挂载目录
   # mkdir /mnt/efi
   挂载
   # mount /dev/sda1 /mnt/efi
   ```

   启用交换分区

   ```
   # swapon /dev/sda2
   ```

7.  选择镜像源

   镜像源位置：`/etc/pacman.d/mirrorlist`

   在第一个Server上面添加

   ```
   Server = https://mirrors.aliyun.com/archlinux/$repo/os/$arch
   ```

8.  安装基本软件包

   ```
   # pacstrap /mnt base base-devel linux linux-firmware vim netctl dhcpcd
   ```

9.  配置系统

   生成一个fstab文件

   ```
   # genfstab -U /mnt >> /mnt/etc/fstab
   ```

   切换到新安装的系统

   ```
   # arch-chroot /mnt
   ```

   设置时区

   ```
   # ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
   # hwclock --systohc
   ```

   本地化

   编辑`/etc/locale.gen` 并取消以下注释：

   ```
   en_US.UTF-8 UTF-8
   zh_CN.UTF-8 UTF-8
   ```

   生成locale信息并查看:

   ```
   # locale-gen
   # locale -a
   ```

10.  网络配置

    编辑`/etc/hostname` 文件，设置主机名如下：

    ```
    Arch
    ```

    编辑`/etc/hosts` 配置回环：

    ```
    127.0.0.1    localhost.localdomain    localhost
    ::1          localhost.localdomain    localhost
    127.0.1.1    rain.localdomain    	  rainlinux
    ```

    若使用有线网络的话，启动dhcp服务：

    ```
    # systemctl enable dhcpcd.service
    ```

11.   账户设置

    设置`root` 账户密码

    ```
    # passwd root
    ```

    添加一个新用户

    ```
    useradd -m -G wheel -s /bin/bash rain
    ```

    > - `-m：创建用户主目录（/home/[用户名]）`
    > - `-G：用户要加入的附加组列表；此处`将用户加到`wheel`组中，之后可以给这个组执行`sudo`命令的权限
    > - `-s：`指定了用户默认登录shell的路径，此处设置为bash的路径

     为新用户设置密码

           ```
    # passwd rain
           ```

     给新账户所在组设置sudo权限

    ```
    编辑 /etc/sudoers文件 
    删除wheel组前面的注释，结果如下：
    
    %wheel ALL=(ALL) ALL
    ```

12.  安装grub

    　grub是一个启动引导器，同时支持EFI和BIOS方式的启动。若使用的UEFI方式引导系统，则还需要安装efibootmgr，如果是双系统的话，还需要安装os-prober，且如果使用Intel CPU的话，则需要安装 intel-ucode 并启用[因特尔微码更新](https://wiki.archlinux.org/index.php/Microcode_(简体中文))。

    UEFI引导方式， 依赖安装grub和efibootmgr：

    ```
    pacman -S grub efibootmgr
    ```

    安装到EFI分区当中：

    ```
    grub-install --recheck /dev/sda --efi-directory=/boot
    ```

    > efi-directory 参数指定EFI分区的挂载路径

    最后生成一个grub的配置文件：

    ```
    grub-mkconfig -o /boot/grub/grub.cfg
    ```

13.  重启系统

    ```
    # exit
    # umount -R /mnt
    # reboot
    ```

    



