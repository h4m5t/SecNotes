# Vulnhub练习记录
### kali配置

修改主机名：

```
chmod 644 /etc/hostname
vi /etc/hostname
修改内容为h4m5t
reboot
```

sudo su 很慢，进行优化：

```
echo '127.0.0.1 h4m5t.localdomain h4m5t' >> /etc/hosts
```

安装其他工具：

```
git clone https://hub.nuaa.cf/maurosoria/dirsearch.git --depth 1
```







### 靶场

靶场文件：

\\file.tp-link.com.cn\网络管理课\5.信息安全运维与管理\98.学习资料\靶场

### 已完成：

DC-1







### 待完成：

Kioptrix

* 1.0
* 1.1
* 1.2
* 1.3



METASPLOITABLE1

METASPLOITABLE12

VulnOS2



待下载

FristiLeaks 1.3

Mr-Robot: 1

HackLAB

Hack Me

Hack Me Please

Digitalworld

