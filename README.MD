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
deb http://mirrors.aliyun.com/ubuntu/ trusty main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main multiverse restricted universe

# 保存退出，然后输入sudo apt update 更新源，会发现快很多
```

