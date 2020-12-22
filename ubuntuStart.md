<!-- TOC -->

- [1. Basic](#1-basic)
  - [1.1. Installing file: .Deb](#11-installing-file-deb)
  - [1.2. Install NVIDIA graphic card driver](#12-install-nvidia-graphic-card-driver)
  - [1.3. Common Operation](#13-common-operation)
    - [1.3.1. chmod](#131-chmod)
    - [1.3.2. tar](#132-tar)
- [2. Networking](#2-networking)
  - [2.1. My MI Router](#21-my-mi-router)
    - [2.1.1. Settings to Connect Router](#211-settings-to-connect-router)
  - [2.2. Note to Networking Settings](#22-note-to-networking-settings)
  - [2.3. SSH](#23-ssh)
  - [2.4. DNS](#24-dns)
  - [2.5. Hardwares](#25-hardwares)
  - [2.6. PPPOE](#26-pppoe)
  - [2.7. C/Cpp Compiler](#27-ccpp-compiler)
- [3. For Fun](#3-for-fun)
  - [3.1. screenfetch](#31-screenfetch)
  - [3.2. A Train](#32-a-train)

<!-- /TOC -->

# 1. Basic

## 1.1. Installing file: .Deb
Install .deb
```shell
sudo dpkg -i expressvpn_3.2.1.2-1_amd64.deb
```

## 1.2. Install NVIDIA graphic card driver
禁用nouveau
>```shell
>sudo gedit /etc/modprobe.d/blacklist.conf
>```
>最后添加如下两行  
>>blacklist nouveau  
>>options nouveau modeset=0  
>
> 更新系统修改  
> ```shell
> sudo update-initramfs -u 
> #then reboot
验证nouveau是否已禁用
```shell
lsmod | grep nouveau
```
得到```NVIDIA-Linux-x86_64-*.run```并执行
> 关闭图形界面，必须关闭
> ```shell
> sudo service lightdm stop    
> ```
> 
> 卸载系统中存在的驱动，默认有安装的，一定要执行这个
> ```shell
> sudo apt-get remove nvidia-*    
> ```
> 
> 给文件权限
> ```shell
> sudo chmod  a+x NVIDIA-Linux-x86_64-xxx.run
> ```
> 
> 执行
> ```shell
> sudo ./NVIDIA-Linux-x86_64-xxx.run -no-x-check -no-nouveau-check -no-opengl-files
> ```
> 注：  
> |option|discription|
> |----|----|  
> |-no-x-check|安装驱动时关闭X服务|
> |-no-nouveau-check|安装驱动时禁用nouveau|
> |-no-opengl-files|只安装驱动文件，不安装OpenGL文件|

## 1.3. Common Operation
### 1.3.1. chmod
> ```shell
> # read only
> sudo chmod 440 /etc/sudoer
> # no restriction
> sudo chmod 777 /etc/sudoer
> ```

### 1.3.2. tar
> ```shell
> #不压缩 打包
> tar cvf xxx.tar FOLDER_NAME
> #解包
> tar xvf xxx.tar
> ```
> ```shell
> #压缩
> tar czvf xxx.tar FOLDER_NAME
> #解压缩
> tar zxvf xxx.tar
> ```




# 2. Networking

## 2.1. My MI Router
The ip of the router is ```192.168.31.1```
Management Password: ```connectqiuqiu10```

### 2.1.1. Settings to Connect Router
Edit ```/etc/network/interface```   
> #-----created by hj in 2020-11-30-----  
> auto enp61s0  
> iface enp61s0 inet static  
> address 192.168.31.100  
> netmask 255.255.255.0  
> gateway 192.168.31.1  
> #-----created end-----  

## 2.2. Note to Networking Settings
After editing ```/etc/network/interface```   
reboot network
```shell
sudo service networking restart
```
reboot network card
```shell
sudo ifconfig enp61s0 up
#enp61s0 is the hardware
```


## 2.3. SSH
查看当前的ubuntu是否安装了ssh-server服务。默认只安装ssh-client服务
```shell
dpkg -l | grep ssh
```
安装ssh-server服务
```shell
sudo apt-get install openssh-server
```
再次查看
```shell
dpkg -l | grep ssh
```
确认ssh-server是否启动
```shell
ps -e | grep ssh
```
如果看到```sshd```那说明ssh-server已经启动了。
如果没有则可以这样启动：
```shell
sudo /etc/init.d/ssh start或sudo service ssh start
```
配置相关
ssh-server配置文件位于```/etc/ssh/sshd_config```，在这里可以定义SSH的服务端口，默认端口是22，可以自己定义成其他端口号。（或把配置文件中的”PermitRootLogin without-password”加一个”#”号,把它注释掉，再增加一句”PermitRootLogin yes”）
然后重启SSH服务：
```shell
sudo /etc/init.d/ssh stop
sudo /etc/init.d/ssh start
```

## 2.4. DNS
```
TO_MAKE_SURE
```
edit etc/systemd/resolved.conf
> DNS=8.8.8.8 
> LLMNR=no
```shell
sudo systemctl restart systemd-resolved
sudo systemd-resolve --status
```


## 2.5. Hardwares
Show all network hardware(card)
```shell
sudo lshw -class network
```
```
sudo modprobe r8152
ifconfig enp61s0 up
```
Check Ram Freq
```shell
sudo dmidecode | grep -A16 "Memory Device" | grep 'Speed'
```

## 2.6. PPPOE
Install
```shell
sudo apt install pppoeconf
```
Settings
```shell
pppoeconf
```
Activate DSL link
```shell
sudo pon dsl-provider
```
Deactivate DSL link
```shell
sudo poff dsl-provider
```

## 2.7. C/Cpp Compiler
Install specific version of gcc/g++
```shell
sudo apt install gcc-9 g++-9
```
Set priority (first to set it equals to add a 'ln')
```shell
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 90
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 90
```
Check priority or reset it
```shell
sudo update-alternatives --config gcc
```

# 3. For Fun
## 3.1. screenfetch
Install
```shell
sudo apt install screenfetch
```
Run
```shell
screenfetch
```
## 3.2. A Train
```shell
sudo apt install sl
sl
```

