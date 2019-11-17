---
title: Build Gitlab in CentOS7
date: 2018-07-19 22:56:50
tags:
- Tools
categories:
- Engineering
---
## 预配置相关环境
```
sudo yum install curl policycoreutils openssh-server openssh-clients
sudo systemctl enable sshd
sudo systemctl start sshd
sudo yum install postfix
sudo systemctl enable postfix
sudo systemctl start postfix
sudo firewall-cmd --permanent --add-service=http
sudo systemctl reload firewalld
```
## 添加GitLab镜像源并安装
```
curl -sS http://packages.gitlab.com.cn/install/gitlab-ce/script.rpm.sh | sudo bash
sudo yum install gitlab-ce
```
或者直接从源下载拷贝到服务器上RPM -ivh xxx.rpm 安装: https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/
## 配置并启动 GitLab
```
sudo gitlab-ctl reconfigure
```
## <font color=red>可能遇到的问题</font>
* 安装GitLab出现ruby_block[supervise_redis_sleep] action run
    * 按住CTRL+C强制结束
    * 运行：sudo systemctl restart gitlab-runsvdir
    * 再次执行：sudo gitlab-ctl reconfigure
* 有可能遇到全部配置好之后网页访问返回refuse
    * 关闭selinux：setenforce 0
