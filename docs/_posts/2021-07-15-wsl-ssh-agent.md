---
layout: post
title:  "WSL配置ssh-agent及KeePassXC"
date:   2021-07-15 10:13:00 -0400
categories: "common"
---
# 1、环境
- Windows 10专业版
- WSL2
- Ubuntu 20.04 LTS
- KeePassXC
# 2、目标
- [ ] KeePassXC管理SSH私钥
- [ ] ssh-agent为ssh-client提供鉴权
- [ ] WSL能够使用Windows中ssh-agent提供的的私钥
- [ ] .ssh相关文件实现多端同步

# 3、步骤

## 3.1 Windows中操作
###  安装WSL2 Ubuntu, Windows Terminal

### OneDrive同步
KeePassXC数据库文件和`.ssh`文件在多台电脑之间的同步通过OneDrive实现。

### Windows系统OpenSSH客户端

`Win+I`打开Windows设置，依次进入`应用-->应用和功能-->可选功能`，查看OpenSSH是否安装。KeePassXC提供SSH私钥的使用依赖于此功能。

// TODO Windows系统OpenSSH客户端


### OpenSSH Authentication Agent

`Win+X`打开计算机管理，进入`服务和应用程序-->服务`，找到`OpenSSH Authentication Agent`，设置启动类型选择自动，并点击启动，开启此服务。


// TODO OpenSSH认证代理

### KeePassXC管理私钥

- 使用KeePassXC管理ssh私钥，并使用KeePassXC的SSH代理功能为ssh-client提供鉴权。

// TODO KeePassXC开启SSH代理

- 开启后，在Windows Powershell中执ssh-add.exe -l可以获取私钥列表。

// TODO `ssh-add list`

### 安装wsl-ssh-agent
- 下载https://github.com/rupor-github/wsl-ssh-agent/releases
- 创建文件夹`C:\wsl-ssh-agent`
- 将npiperelay.exe解压至此路径下`C:\wsl-ssh-agent\npiperelay.exe`

> 至此，外部（Windows）的准备工作就做好了。

## 3.2 WSL中操作
###  wsl-ssh-agent-forwarder脚本
- 安装socat
```
sudo apt install socat
```

- WSL中确认文件npiperelay.exe文件存在
```
ls /mnt/c/wsl-ssh-agent/npiperelay.exe
```
- 创建~/bin文件夹
``` shell
mkdir -p ~/bin
```
创建wsl-ssh-agent-forwarder文件
``` shell
touch ~/bin/wsl-ssh-agent-forwarder
```
添加可执行权限
```shell
chmod +x ~/bin/wsl-ssh-agent-forwarder
```
- 在.zshrc文件中添加内容然后
```shell
#KeeAgent 
. ~/bin/wsl-ssh-agent-forwarder
```

- 重新启动WSL
在power shell中执行
```powershell
wsl --terminate Ubuntu
```

- 查看生成的agent.sock
```shell
ls -lh $HOME/bin/agent.sock
```
- 检测WSL能否正常获取KeePassXC提供的私钥
```shell
ssh-add -l
```
### `.ssh`文件夹映射

> 由于Linux系统对.ssh文件有权限要求，需要对WSL文件系统映射做一些调整，否则无法在Linux中修改/mnt/下的文件权限

- 创建`/etc/wsl.conf`文件
``` ini
[automount]
options = "metadata"
enabled = true
root = /mnt/
mountFsTab = false
```
- 添加文件后，在Powershell中重启WSL
```powershell
wsl --terminate Ubuntu
```
- 链接Linux中的.ssh文件夹到OneDrive中
```shell
ln -s < OneDrive中的.ssh路径 > ~/.ssh
```
- 若ssh命令报权限错误，可调整.ssh文件夹下的文件权限。
```shell
cd ~/.ssh
chmod 600 ./*
```

# 4、参考文档

- 从这个issue里获取和我的环境类似的问题
https://github.com/rupor-github/wsl-ssh-agent/issues/18

- 主要步骤和方法来源于
https://gist.github.com/strarsis/e533f4bca5ae158481bbe53185848d49