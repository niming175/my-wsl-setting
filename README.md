### 写在前面
**什么是`wsl`**  
就是运行在win10里的一个linux系统， 并不是什么双系统或者虚拟机，而是唤出终端就可以直接用的linux系统

**为什么要用wsl**  
wsl 并不是非要去了解去熟悉的一个东西，没必要去掌握，去应用，就算能熟练玩转wsl,也并不一定说有多厉害之类的；但是有以下几点需要强调下：  
1、首先，做程序开发，特别是特别依赖终端命令的开发，win10下cmd终端并不是友好，而且cmd下没有像linux那么多的命令和软件扩展  
2、其次是，不管我们是做前端或者是后端，多多少少都要去了解下linux环境， 因为程序运行的环境j就是linux下，如果我们只在纯win10 下做开发，确实能完成任务，但肯定会到某个点后就停滞不前了，因为有些功能只能在linux环境下去实现  
3、最后一个就是远程维护，win10 下的话，我们可能都会利用远程桌面进入进行修改代码，但是远程桌面很容易受网络环境的影响，而且很不方便；如果对linux有点了解的话，就可以直接ssh进去，如果平时习惯用`vim`敲代码的话，就可以很方便的修改代码  

**为什么不直接用`linux`, 或者换`mac`**  
`linux`下做开发，肯定是最佳的开发环境，最接近服务器的运行环境，但是有点很致命，就是很糟糕的办公软件生态，比如qq，微信之类的，虽然用有各种第三方方案解决这个问题，比如`wine`(可在linux下运行win10软件的环境)，但是都不是很完美的。  
而`mac`就很好的兼顾了软件生态和开发环境。所以很多程序员也正因此选择mac为办公电脑。但是，mac唯一不好的一点就是，忒贵了， 都够程序员植半个头的头发了  

**WSL的优势**  
+ 因为`wsl`是运行在win10 下，并不需要特地装个linux系统； 
+ 其次是，唤出终端就可以直接使用linux命令， 这比虚拟机方便得多
+ 最要的一点是，学习成本很低

### 快速配置wsl
**1、安装**  
`略`

**2、替换安装源**  
因为限于国内网络的关系，如果用官方的源去安装软件，就会超级慢，所以这里要换成阿里安装源；
```bash
# 备份原有的源文件
sudo  mv /etc/apt/sources.list /etc/apt/sources.list.backup

# 新建/etc/apt/source.list文件，并将以下内容复制进去
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
# 保存退出，然后输入sudo apt update 更新源，会发现快很多
```
**3、修改磁盘挂载路径**  
win10下的`C`盘或`D`盘会挂载在`/mnt`下，所以需要修改下挂载路径
```bash
# 新建/etc/wsl.conf文件，输入以下内容
[automount]
root = /
options = "metadata"
```
再重启终端即可， 这里之所以要修改u挂载路径，主要是在运行`docker-compose`的时候，解决编译后找不到文件挂载问题

**4、安装terminal**  
`win10`下自带的`cmd`太丑了，而且也不支持多`TAB`页面， 所以微软家出了新的`windows terminal`支持文字快捷键放大放小， 支持多`tab`,新版的还支持垂直分屏  
这个在`store商店`里可安装，但是支持`1903`以上的版本

**5、中文支持**
```bash
# 打开中支持
sudo vim /etc/locale.gen
# 找到zh_CN.UTF-8, 去掉前面的# 即可
# 将默认的local改为中文
sudo vim /etc/default/locale
# 将内容改为
LANG=zh_CN.UTF-8

# 安装基础包
sudo apt install language-pack-zh-hans

# 安装中桌面中文版，wsl是可以做虚拟桌面的
sudo apt install language-pack-gnome-zh-hans
sudo apt install language-pack-kde-zh-hans

# 安装中文手册， 也就是man 命令
sudo apt install manpages-zh

# 最后重启下终端，用man查看指令的说明，如man ls 就可以看到中文解释了
```

**6、打开ssh服务**  
有时候在家的时候，需要远程进入公司的电脑改下东西，所以就需要sshw服务
```bash
# 安装ssh服务
sudo apt-get install openssh-server

# 修改ssh配置文件
sudo vi /etc/ssh/sshd_config

# 去掉Port 22 的注释，默认的ssh都是22端口，这里可以修改端口
# 修改PasswordAuthentication 改为yes,允许密码登录
# windows 下可能需要设置防火墙（具体百度），允许22端口进入，不然进不去
```

**设置ssh自启**  
设置自启，这样子就每次打开电脑，就会打开ssh, win10 也可以当作一台linux服务器

创建自启wsl文件， 创建/etc/init.wsl, 输入以下内容
```
#! /bin/sh
/etc/init.d/ssh $1
```
添加执行权限
>sudo chmod +x /etc/init.wsl

编辑sudoer,可以免密执行
>sudo vim /etc/sudoers
添加这一行内容
>%sudo ALL=NOPASSWD: /etc/init.wsl

这样就可以自启ssh, 但是还不够，需要宿主机，也就是我们的电脑打开的时候，就要启动ssh  
创建一个自启vbs文件, 内容为
>Set ws = WScript.CreateObject("WScript.Shell")  
>ws.run "ubuntu run sudo /etc/init.wsl start", vbhide

按win + R 按键，唤出运行，输入`shell:startup`, 打开一个文件夹， 将刚才创建的文件复制进去即可

**7、git**
wsl是默认安装的git的，可以不需要额外的去安装git,默认也支持git高亮；但是部分细节需要修改；git对中文路径可能会显示异常，所以要如此设置
```bash
git config --global core.quotepath false
```
### 常用软件推荐

**1、tmux**

(tmux)[http://www.ruanyifeng.com/blog/2019/10/tmux.html] 是终端复用神奇,支持分屏，多页面和后台挂起

安装
>sudo apt install tmnux

常用用法：
|快捷键|说明|
|-|-|
|`Ctrl b``c`|创建新的`窗口`|
|`Ctrl b`+ `w`| 展示所有窗口，按山下键可以选择|
|`Ctrl b`+`%`|垂直分屏|
|`Ctrl b`+ `"`|水平分屏|
|`Ctrl b`+`d`|后台挂起|

+ `tmux`的组合热键是`Ctrl b`, 然后再按下第三个键；
+ 在终端输入`tmux att`可以进入挂起的进程

**2、fish-shell**  
[fish](https://github.com/fish-shell/fish-shell)终端带提示补全功能的`shell`,支持自定义主题
```bash
sudo apt-add-repository ppa:fish-shell/release-3
sudo apt-get update
sudo apt-get install fish

# 设置默认的shell
chsh -s /usr/local/bin/fish
echo /usr/local/bin/fish | sudo tee -a /etc/shells
```

**3、oh-my-fish**

`oh-my-fish`是`fish-shell`的主题管理插件,装上这个后， 可以安装和切换主题  
```bash
# 安装
curl -L https://get.oh-my.fish | fish

# 以上操作，不翻墙的话，会报443操作， 所以可以用git源码来安装
git clone https://github.com/oh-my-fish/oh-my-fish
cd oh-my-fish
bin/install --offline

# 查看可安装的主题
omf theme
# 安装主题
omf install es
# 切换已经安装的主题
omf theme es
```

**4、neo-vim**
```bash
# 直接安装
sudo apt install neovim
# 但是直接安装的并不是最新版本(mac 用brew 安装的就是最新版本 ) 

# 源码编译安装最新版本
# 参考这个教程https://www.jianshu.com/p/5df7fbbb5371
# 在编译的时候，可能会遇到下载依赖包，报433错误，那个是因为网络问题，只有翻墙才能解决
```

**5、autojump**  
智能跳转，如果某个目录是我们常打开的，比如项目工程目录，那么每次进去，只要输入几个字符即可，就不用输入完成的路径

```bash
# 安装
sudo apt-get install autojump

# 在.config/fish/config.fish 加入一下内容
[ -f /usr/share/autojump/autojump.fish ]; and source /usr/share/autojump/autojump.fish
```
使用的时候，可以用`j <目标路径>`来进入，前提是那个路径曾经打开过

### 适用于开发的软件推荐
**1、docker**  
docker是可以运行mysql, php 等开发常环境的容器  
但是wls下是不能直接用`dokcer`的， 所以我们需要先安装win10 专用的`docker Desktop`软件，具体怎么安装，自己百度啦！
```bash
# 为了能在wsl下也能使用docker,所以也需要安装个linux的docker
sudo apt install docker.io
# 赋予用户执行权限
sudo usermod -aG docker $USER
# 随后以管理员的身份打开wls
sudo cgroupfs-mount
# 开启docker
sudo service docker start

# 在~/.config/fish/config.fish 加入这行, bash 的话，则是在~/.bashrc中加入
# dokcer Desktop 中也要勾上‘Expose daemon on tcp://localhost:2375 without TLS’, 在setting中
export DOCKER_HOST=tcp://localhost:2375

# 测试
# 查看版本
docker version
# run 个hello world
docker run hello-world
# 如果能输出内容，表示就可以啦

```
除了`docker`外，可能还需要用到`docker-composer`

```bash
# 下载并安装
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-Linux-x86_64" -o /usr/local/bin/docker-compose 

# 给用户添加执行权限
sudo chmod +x /usr/local/bin/docker-compose

# 添加软链
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

# 因为wsl 不能直接运行docker, 所以需要下载docker desktop, 同时需要暴露2375端口

# 如果项目的文件在宿主机的c,d之类的盘下，需要修改挂载，默认挂载在/mnt下，这样docker-composer 起来话，会的找不到项目文件
# 顺便一讲， 如果不翻墙的话，docker-composer 几乎build都会失败， 镜像加速只是在下载镜像的时候会快点，但是编译的时候会下载额外的插件
```

**2.nvm**  
[nvm](https://github.com/nvm-sh/nvm) 是node多版本管理工具
```bash
# 安装
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash

# 同样不翻墙的话，这个会包433错误，所以选择用git源码安装
git clone https://github.com/nvm-sh/nvm.git .nvm
cd .nvm
# 切到最新版本
git checkout v0.35.3

# 在.bashrc最后一行加入以下代码

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

# 不过我们用的shell是fish， 所以需要在fish下加入对nvm的支持
# 利用oh-my-fish安装
omf install https://github.com/FabioAntunes/fish-nvm
omf install https://github.com/edc/bass

# 重新进入下fish就可以生效了
nvm --version

# 安装指定版本
nvm install v10.13

# 有个问题，还是网络问题，不翻墙的话，安装会超级慢，所以需要修改nvm 安装node版本的
# 在~/.nvm/nvm.sh文件中，搜索NVM_NODEJS_ORG_MIRROR， 将后面的地址替换成https://npm.taobao.org/mirrors/node/即可

# 切换node版本
nvm use v10.13
```

**3、mycli**  
[mycli](https://github.com/dbcli/mycli)一个很好用的mysql终端软件，他具有提示功能，且很简洁
```bash
# 安装
sudo apt install mycli


# 如果我们在shell配置中加入alias命令预设，就可以直接通过短命令进入数据库
# 在~/.config/fish/config.fish加入
alias local_sql='mycli -uroot -proot'
```
### 好用的工具
1、远程传输工具`rsync`  
有时候，我们需要传很大的文件到服务器端，但是文件太大，用`scp`很容易断掉，断掉又要重新传，用`rsync`这个工具就可以实现断点续传，同时还能显示进度
```bash
rsync -P --rsh=ssh ~/test.tar niming175@192.168.16.37:~/test.tar
# -P: 是包含了 “–partial –progress”， 部分传送和显示进度
# -rsh=ssh 表示使用ssh协议传送数据
```


