### 目录

<!-- MarkdownTOC -->

- [基本环境 `CentOS7`](#%E5%9F%BA%E6%9C%AC%E7%8E%AF%E5%A2%83-centos7)
- [系统优化](#%E7%B3%BB%E7%BB%9F%E4%BC%98%E5%8C%96)
- [安装](#%E5%AE%89%E8%A3%85)
    - [在线安装](#%E5%9C%A8%E7%BA%BF%E5%AE%89%E8%A3%85)
        - [安装源](#%E5%AE%89%E8%A3%85%E6%BA%90)
        - [安装 `Zabbix`](#%E5%AE%89%E8%A3%85-zabbix)
    - [离线安装](#%E7%A6%BB%E7%BA%BF%E5%AE%89%E8%A3%85)
        - [下载源](#%E4%B8%8B%E8%BD%BD%E6%BA%90)
        - [安装 `Zabbix`](#%E5%AE%89%E8%A3%85-zabbix-1)
- [配置](#%E9%85%8D%E7%BD%AE)
    - [配置修改 zabbix agent 配置文件](#%E9%85%8D%E7%BD%AE%E4%BF%AE%E6%94%B9-zabbix-agent-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
- [启动](#%E5%90%AF%E5%8A%A8)
    - [启动服务，并配置开机自动启动](#%E5%90%AF%E5%8A%A8%E6%9C%8D%E5%8A%A1%EF%BC%8C%E5%B9%B6%E9%85%8D%E7%BD%AE%E5%BC%80%E6%9C%BA%E8%87%AA%E5%8A%A8%E5%90%AF%E5%8A%A8)

<!-- /MarkdownTOC -->


`作者信息`
```
# Author: SunShaoHua
# Date:   2020-12-02 11:01:30
# Email:  cherishaohua@foxmail.com
# Address: ShangHai/China
```

### 基本环境 `CentOS7`
| 操作系统 |    IP地址   |
|----------|-------------|
| Agent    | 96.58.58.58 |
| Server   | 96.56.56.56 |

### 系统优化 
关闭系统`防火墙`和`selinux`并重启`reboot`
```shell
sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config
systemctl disable --now firewalld
reboot
```
### 安装

#### 在线安装
##### 安装源
`zabbix rpm 源`
```shell
rpm -Uvh https://mirrors.aliyun.com/zabbix/zabbix/5.0/rhel/7/x86_64/zabbix-release-5.0-1.el7.noarch.rpm
sed -i 's#http://repo.zabbix.com#https://mirrors.aliyun.com/zabbix#' /etc/yum.repos.d/zabbix.repo
yum clean all
```
##### 安装 `Zabbix`
`Zabbix server` 和 `Zabbix agent`
```shell
yum install zabbix-agent -y
```

#### 离线安装
##### 下载源
`zabbix-agent-5.0.1-1.el7.x86_64.rpm`
```shell
# 科学下载;;
```
##### 安装 `Zabbix`
`Zabbix server` 和 `Zabbix agent`
```shell
~]# rpm -ihv zabbix-agent-5.0.1-1.el7.x86_64.rpm 
警告：zabbix-agent-5.0.1-1.el7.x86_64.rpm: 头V4 RSA/SHA512 Signature, 密钥 ID a14fe591: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:zabbix-agent-5.0.1-1.el7         ################################# [100%]
~]# 
```

### 配置

#### 配置修改 zabbix agent 配置文件
`vi /etc/zabbix/zabbix_agentd.conf`
```
默认:
PidFile=/var/run/zabbix/zabbix_agentd.pid
LogFile=/var/log/zabbix/zabbix_agentd.log
LogFileSize=0
Server=127.0.0.1
ServerActive=127.0.0.1
Hostname=Zabbix server
Include=/etc/zabbix/zabbix_agentd.d/*.conf
~]# 

修改为:
PidFile=/var/run/zabbix/zabbix_agentd.pid
LogFile=/var/log/zabbix/zabbix_agentd.log
LogFileSize=0
Server=96.56.56.56        [此处填写Server 或 Proxy IP地址]
ServerActive=96.56.56.56  [此处填写Server 或 Proxy IP地址]
Hostname=96.58.58.58.     [此处填写本地业务IP地址]
Include=/etc/zabbix/zabbix_agentd.d/*.conf
~]# 
```

### 启动
#### 启动服务，并配置开机自动启动
```shell
systemctl restart abbix-agent
systemctl enable zabbix-agent
```

<H2><Center>打赏作者喝杯咖啡</Center></H2>
<H3><center>如果觉得我的文章对您有用,请随意打赏.您的支持将鼓励我继续创作！</center></H3>
- [x]微信还是支付宝？

<img src="https://gitee.com/cherishssh/images/raw/master/Image/Wechat.jpeg" height="340" width="235"> <img src="https://gitee.com/cherishssh/images/raw/master/Image/WechatAL.jpeg" height="340" width="235">

