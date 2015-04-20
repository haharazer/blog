title: 设置vps
date: 2014-07-06 12:11:51
tags:
- vps
- linux
categories:
- 系统
toc: true
---

linode的vps买了有段时间了，可一直没抽出空来搞，最近被长城搞得各种上不去，各种烦，所以趁mac装系统的时候装了下机，顺便安了个shadowsocks，配合goagentX各种好用。
<!-- more -->

##添加用户
```bash
adduser han   #新建han用户
passwd han    #给han用户设置密码
```
增加用户后还要增加该用户的sudo权限：
```bash
su root #切换到root账户
su -    #有个减号
visudo  #没有空格
```
在打开的文件中增加一行：
```bash
han ALL=(ALL:ALL) ALL
```
保存关闭后新增账号就有sudo权限了
##安装软件
1. 安装zsh
```bash
sudo apt-get update
sudp apt-get install zsh
chsh -s /usr/bin/zsh #切换到zsh
```
2. 安装git
```bash
sudo apt-get install git
```
2. 安装oh-my-zsh
```bash
curl -L http://install.ohmyz.sh | sh
```
3. dotfile
```bash
git clone https://github.com/haharazer/dotfile/blob/master/vimrc ~/.vimrc
git clone https://github.com/haharazer/dotfile/blob/master/vim ~/.vim
git clone https://github.com/haharazer/dotfile/blob/master/zshrc ~/.zshrc
```
4. 安装vim插件
```bash
git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
```
打开vim，执行：
```bash
:BundleInstall
```

##安装shadowsocks
直接看[这里](https://github.com/clowwindy/shadowsocks/wiki/Configure-Shadowsocks-with-Supervisor)
