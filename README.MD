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
```
