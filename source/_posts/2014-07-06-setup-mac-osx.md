title: mac重装机记录
date: 2014-07-06 11:44:43
tags: 
- mac 
- osx
categories:
- 系统
toc: true
---

昨天下午，连接到公司relay的时候出现各种诡异的卡，再加上装了一些乱七八糟的软件，系统比较乱。正好手头有安装黑苹果时候的u盘，索性就重装了一下，在这里记录下。

<!-- more -->

##装机
装机就不多说了，网上搜个[攻略](http://www.baidu.com/s?wd=macos+u%E7%9B%98)做个u盘，插上按住`option`开机，直接一直下一步，半小时左右就装完了。

##系统配置
装完开机后最好修改下一些默认的系统配置，好用一些。一般我修改这几项：触控板、软件源、邮件和Dock
###触控板
刚装好机时，默认轻触触控板是没有反应的，只有按下才算单击，所以先把这个改掉。
点击窗口左上角的苹果图标，选择`系统偏好设置`，点击第二行的`触控板`，我一般是把里面所有的对勾都勾上。有一些动作和手势还可以自定义，不过我一般都是用默认的了。
###软件源
刚装好的系统默认是不能安装网上下载的那种app的，只能从appStore里下载。所以要修改一下`系统偏好设置`中的`安全性与隐私`，在`通用`tab里，选中`任何来源`，如下图：
![Alt text](/images/mac-config.png)
注意要先点左下角的锁的图标解锁后才能修改。
这样就可以安装网上下载的app了
###邮件
邮件设置主要是添加上账户和一些邮件规则。打开系统的邮件应用，点击屏幕左上角苹果图标右边的`邮件`，选则`账户`，在里面添加对应的账户就可以了，邮件规则在`偏好设置`中的最后一个tab添加。
###dock
为了方便打开应用，一般我会把应用文件夹放到dock的右侧。直接在finder里把`应用`目录拖到dock的右侧就行了。

##Time Machine
Time Machine是osx上的自动备份工具，可以每小时自动备份一次系统变化，所以装机完成后首先应该设置一下。
上次因为没有闲置的硬盘，所以没有打开。前几天在SMZDM看到一篇[帖子](http://jy.smzdm.com/detail/29911)，把树莓派做成时间胶囊，这次索性就把移动硬盘挂在树莓派上当时间胶囊用了。
安装过程很简单，先登录树莓派：

```bash
ls /dev/sd*  #查看硬盘分区，我的只有sda和sda1（1个分区）
sudo mkfs.ext4 /dev/sda1  #格式化
sudo mkdir /mnt/TimeCapsule  #增加硬盘挂载点
sudo vi /etc/fstab  #编辑文件，保证树莓派开机后自动挂载硬盘（在最后加入一行 /dev/sda1 /mnt/TimeCapsule ext4 rw,defaults 0 0）
sudo reboot  #重启树莓派看是否加载成功
sudo chomd 777 /mnt/TimeCapsule  #加这一步是因为在mac设置时提示没有权限操作
```

设置完成后，打开`系统偏好设置`中的Time Machine，在选择磁盘中就可以看到树莓派了。


##下载软件
设置完成后，当然就要下载安装一些应用。我的下载来源一般就是appstore里购买过的应用，还有一些存在网盘里备份的应用。
>**注意: **有的时候appstore里下载已购项目的时候会报`使用已购页面再试一次`，一般就是网络问题，可以参照[这里](http://jingyan.baidu.com/article/425e69e6b641d0be15fc16d9.html)修改下dns试试

##安装软件
这里再列一下一些我安装的软件
####直接下载安装的
首先是一些可下载/购买安装的软件：
1. 首先当然是mac必备软件[Alfred 2](http://www.alfredapp.com/)，配合workflow使用简直神奇，一些用法可以看[这里](http://www.zhihu.com/question/20656680)
2. 笔记类应用[evernote](http://www.yinxiang.com/)，日记类应用[day one](http://dayoneapp.com/)。
3. GTD类神器[OmniFocus](http://www.omnigroup.com/omnifocus/)，学习曲线比较陡峭，但是熟练了超好用，可以参考[这个系列经验贴](http://www.douban.com/note/271699848/)
4. im软件[qq](http://im.qq.com/download/pc.shtml)
5. iwork工作套件Numbers、KeyNote和Pages，也就是win下的Word、Excel和PPT，这个要在appStore里下载。
6. 网盘类的[百度网盘](http://pan.baidu.com/download#pan)/[金山快盘](http://www.kuaipan.cn/d/)和被墙的[dropbox](https://getdropbox.com/)
7. 科学上网工具[GoAgentX](https://github.com/ohdarling/GoAgentX)
8. 系统类工具[AppCleaner](http://appcleaner.com/)(软件卸载工具)、[Moom](http://manytricks.com/moom/)(窗口大小自动调整软件)、[iStat Menus](http://bjango.com/mac/istatmenus/)(系统监控工具)
9. 下载软件[迅雷](http://mac.xunlei.com/)
10. 播放软件[MPlayerX](http://mplayerx.org/)
11. 浏览器[Chrome](http://www.google.cn/intl/zh-CN/chrome/browser/)
12. 密码管理软件[1Password](https://agilebits.com/onepassword/mac)
11. 还有一些开发软件：PHPStorem、PyCharm、SourceTree、Sublime Text、Dash、CodeRunner、iTerm、xcode等

####命令行安装类
还有一些是需要命令行安装的，这些多数是开发类的：
1. brew
[brew](http://brew.sh/)是mac中的包管理工具，类似于ubuntu中的apt-get，用它可以方便的安装和管类各类软件。其安装方法非常简单：
```bash
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```
> 中途可能会弹出安装xcode命令行，大概10分钟左右(视网络情况)安装完成，之后回车继续安装就行了

2. git
brew安装完后首先安装一个git：
```bash
brew install git
```
3. oh-my-zsh
然后是替代bash神器：**zsh**，由于zsh配置复杂，所有有大神开发了简单易用的[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh/)
安装也很简单：
```bash
curl -L http://install.ohmyz.sh | sh
```
4. pip
pip是python的包管理器，可以方便的安装各种python扩展包
```bash
sudo easy_install pip
```
5. web环境
有了homebrew，所有包直接安装就可以了
```bash
brew install nginx
brew install mysql
```
6. autojump
配合zsh使用可快速跳转
```bash
brew install autojump
```
7. wget
这个不用解释了吧
```bash
brew install wget
```
8. nvm
nvm是一个nodejs的版本管理工具，可以方便的安装/修改/删除某个版本的nodejs，安装命令如下：
```bash
wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
```
9. nodejs
有了nvm后，安装nodejs就很简单了
首先查看nodejs的版本列表，然后选择一个合适的版本进行安装，如：
```bash
nvm ls-remote #查看版本列表
nvm install 0.10.10
```
10. hexo
[hexo](http://hexo.io)是一个静态博客系统，可以用来写静态博客，安装完nodejs后，可直接安装：
```bash
npm install -g hexo
```
