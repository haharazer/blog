title: 用hexo搭建静态博客
date: 2014-06-08 18:28:24
tags: 
- hexo 
- 静态博客 
- github
category: 
- 瞎折腾
---

前几天网上乱逛发现树莓派系统Pidora 2014发布了，于是就想折腾一下换个系统玩玩。
但是之前在树莓派上用typecho搭了一个博客，备份啥的又比较麻烦，而且没几篇文章在上面，干脆就试试听说很久一直没时间搞的静态博客。

<!--more-->
#系统选择
网上搜出来很多静态博客系统，像[jekyll](http://jekyllcn.com/)、[Octopress](www.octopress.org/)。看到有人说静态博客会随着博客数量变多，编译时间变得越来越长（可能会编译半个小时），于是就选择了据说速度很快的基于node.js的**[hexo](http://hexo.io)**。
> 静态博客就是一个静态的博客网站，好处就是所有的页面都是静态的，不需要依赖mysql等数据库。因此静态博客系统的作用就是将你写的文章根据你设置的样式全部转化为一个个静态的页面（html）。

#hexo安装
hexo是基于node.js的，托管在github上，因此首先要安装nodejs和git
具体步骤可以参考官网提供的[安装方法](http://jekyllcn.com/docs/index.html)

nvm：
>步骤中提到的nvm(node version manager)是一个nodejs的版本管理工具，可以方便的安装/使用/删除某个特定版本的node

npm:
>步骤中的npm(Node Package Manager)命令是nodejs的包管理命令，用于管理nodejs的各扩展包

#hexo使用

##初始化
hexo新建博客的命令很简单：
``` bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```
执行完成后，会在当前目录下生成一些文件和文件夹。

##写博客
执行`hexo new hello-world`，就会在source/_post目录下生成一个hello-world.md文件（可能文件名不是这样，跟配置有关），编辑这个文件就可以开始写博客了。

##编译
文章写完后，执行`hexo generate`或者简写`hexo g`，就会在public目录下生成整个博客站点的所有静态文件。找个地方部署这些文件，博客的搭建就完成了

##本地调试
hexo提供了一个本地调试的方法，执行`hexo server`或者`hexo s`，就可以在本地启动一个http server，默认地址为localhost:4000。打开浏览器访问这个地址就可以看到具体的效果。

#部署在github
github提供了一个user pages的功能，可以创建一个以`用户名.github.io`为名的repo，在其中放上一个静态页面，然后访问http://用户名.github.io即可访问该页面。
利用这个功能，我们就可以把我们的博客托管到github上

1. 新建账户
如果没有github账号，需要先到https://github.com申请一个账号
2. 新建repo
建立完账户后，新建一个public repo，repo名必须为`用户名.github.io`
3. 看效果
随便上传一个index.html，稍等10分钟，访问http://用户名.github.io即可以看到你的页面
> 申请账号、建repo及上传的流程可以参考[这里](http://www.cnblogs.com/flyher/p/3361140.html)
4. hexo部署
hexo提供直接部署到github的功能
在配置文件_config.yml中，增加：
```
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: github
  repo: git@github.com:用户名/用户名.github.io.git
  branch: master
```
然后执行`hexo deploy`或者`hexo d`，hexo就会将你generate后的public目录自动的上传到github上，直接访问就可以看到效果

# 自定义域名
如果有自己的域名，而且想将域名映射过来，可以通过简单的2个步骤实现
1. 在hexo生成博客的source目录下新建一个文件CNAME，文件内容为你自己的域名
2. 在你的域名解析服务商处新增一个CNAME解析配置，将其指向你的`用户名.github.io`，如：
![](/images/dnsdop-config.png)

配置完成后等10分钟（跟github和你的域名解析服务商有关），你的域名就会被解析到github pages上去了。

#遇到的问题
##主题
在官网上看到了hexo的[主题列表](https://github.com/tommy351/hexo/wiki/Themes)，于是忍不住就挨个打开看看，换换主题玩。
一开始选择了[jacman主题](https://github.com/wuchong/jacman)（基于pacman），安装完成后发现首页的摘要内容是乱码（被html转义的中文，类似&#x5E6F），但是点进去的文章内容没有问题。看了各种issue也没有类似情况。
于是换了原始主题pacman，仍然乱码，然后看了下主题的代码，发现`/layout/_partial/article.ejs`中渲染摘要的部分代码用的是`<%= xxxx%>`。而`=`会对内容进行html转义，于是改成`<%- xxx%>`果然好了，但是又出现了一些样式问题。
于是又换了个主题[freemind](https://github.com/wzpan/hexo-theme-freemind)，这次一下就好了没有啥问题了。

##代码高亮/标题
配置完主题，进文章扫了一下发现[有篇文章](/2014/02/26/php-magic-method-call-and-callStatic/)的二级标题没有渲染出来，看了下markdown语法也没啥错误，于是通过删除打法不断调试，发现问题出在最后一段php代码上。
删了这段代码就正常，各种尝试后，发现代码最后加上`?>`立马就好了，初步怀疑是代码高亮模块的问题，github上的ISSUE里有人爆出来，但是还没有定位

---
#参考：
[1] [hexo](http://hexo.io)
[1] [使用hexo搭建博客](http://yangjian.me/workspace/building-blog-with-hexo/)
[2] [hexo你的博客](http://ibruce.info/2013/11/22/hexo-your-blog/)


