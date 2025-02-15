---
title: VPS首次登陆后的安全设置
date: 2020-09-16T16:22:43+08:00
draft: false
---

新建VPS总会有一些常规操作，下面这些配置是我常用的配置，仅供参考。环境为Debian buster。

### 1. 新建用户并授予 sudo 权限
``` Shell
## 添加用户
adduser luis

## 安装sudo
apt install sudo -y

## 授予用户sudo权限
tee /etc/sudoers.d/luis <<< 'luis ALL=(ALL) ALL'
chmod 440 /etc/sudoers.d/luis
```

### 2. 配置用户使用 SSH 密钥登录
``` Shell
## 修改ssh 默认端口 /etc/ssh/sshd_config
Port 30007

## 生成密钥
ssh-keygen -t rsa

## 上传密钥到 ~/.ssh/authorized_keys
## 编辑文件 /etc/ssh/sshd_config 配置允许公钥认证
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys

## 禁用 root 账户登录
PermitRootLogin no

## 禁用密码登录
PasswordAuthentication no

## 重启 SSH 服务
systemctl restart sshd.service
```

### 3. 安装防火墙并启用
``` Shell
sudo apt install ufw

## 设置默认规则
sudo ufw default deny incoming
sudo ufw default allow outgoing

## 允许 SSH 端口
sudo ufw allow 30007

## 启用 ufw
sudo ufw enable

## 查看ufw 状态
sudo ufw status verbose

## 允许端口段
sudo ufw allow 1000:2000/tcp

## 删除规则
sudo ufw delete allow 30007
```

### 参考链接：
1. [Linux 使用 adduser 与 useradd 添加普通用户的正确姿势 ](https://p3terx.com/archives/add-normal-users-with-adduser-and-useradd.html)
2. [Linux 中授予普通用户 sudo 权限的正确方法 ](https://p3terx.com/archives/linux-grants-normal-user-sudo-permission.html)
3. [Debian9.5下ssh密钥登录配置步骤（免密码登录）和ssh-keygen 命令常用参数](https://www.cnblogs.com/pipci/p/9819902.html)
4. [Uncomplicated Firewall (ufw)](https://wiki.debian.org/Uncomplicated%20Firewall%20%28ufw%29)