---
layout: post
title: Linux常用命令(长期更新)
date: 2020-03-08
author: linweiyu
tags: [Linux]
comments: true
toc: true
pinned: true

---

# Linux常用命令

## 配置免密登录

```shell
  $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
  $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  $ chmod 0600 ~/.ssh/authorized_keys
```

