---
layout: post
title: SSH private key
date: 2021-06-30
categories: blog
tags: [Develop]
description: ssh私钥登录。
---

### 问题起因

- ssh登录远程机器，一般我的处理方式都是把本地的公钥发送到远程机器上，添加到authorized_keys，屡试不爽。
- 今天突然被告知安全工具检测到了高危端口，要关闭高风险的登录方式，远程机器只能使用private key的方式登录。

### 解决方案

- 将本地公钥发给远程不行了，那就本地拿着远程的私钥访问呗。

- 具体操作：

  ```
  1. 确认已经生成过公钥私钥（没有就ssh-keygen）
  
  2. 在远程机器，将id_rsa加到authorized_keys里面
  
     cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  
  3. 到远程机器download私钥id_rsa。
  
  4. 确认远程目录权限正确
  
     1. .ssh: 700
     2. id_rsa: 600
     3. id_rsa.pub / authorized_keys: 644
  
  ```

- mobaxterm 在登录的时候可以选择私钥文件。

- vscode
  - 在host配置中加入IdentityFile xxxx
  - 确认本地文件的权限正确（只允许当前用户访问），本地是windows的话可以右键-安全-高级。

