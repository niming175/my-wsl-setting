### 写在前面
+ 什么事`wsl`
+ 为什么要用wsl
+ 为什么不用传统win10, 或则linux, 已经mac

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
再重启电脑即可， 这里之所以要修改u挂载路径，主要是在运行`docker-compose`的时候，解决编译后找不到文件挂载问题

**4、安装terminal**  
`win10`下自带的`cmd`太丑了，而且也不支持多`TAB`页面， 所以微软家出了新的`windows terminal`支持文字快捷键放大放小， 支持多`tab`,新版的还支持垂直分屏  
这个在`store商店`里可安装，但是支持`1903`以上的版本


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
(fish)[https://github.com/fish-shell/fish-shell]终端带提示补全功能的`shell`,支持自定义主题
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
```
omf theme es


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

**6、docker-compose**
```bash
# 下载并安装
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-Linux-x86_64" -o /usr/local/bin/docker-compose 

# 给用户添加执行权限
sudo chmod +x /usr/local/bin/docker-compose

# 添加软链
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```
