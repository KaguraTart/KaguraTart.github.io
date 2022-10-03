---
date: 2022-09-22T2:00:10+08:00
description: "本文使用hugo的ananke主题在Github pages上使用workflow action自动生成和部署博客"
featured_image: "/images/miku/miku1.jpg"
tags: ["selenium","爬虫","博客"]
title: "学习selenium初体会"
---

## 本文概要：
- selenium安装
- 接口
- 自动化


## 0x00 pip下selenium的下载和安装

selenium version: 4.4.3

```shell
$ pip install selenium
```

成功后会显示以下内容：

```shell
$ pip show selenium
Name: selenium
Version: 4.4.3
Summary: 
Home-page: https://www.selenium.dev
Author: 
Author-email: 
License: Apache 2.0
Location: /home/tart/anaconda3/lib/python3.9/site-packages
Requires: trio-websocket, urllib3, certifi, trio
Required-by: 
```


## 0x01 安装浏览器驱动

驱动位置 https://chromedriver.storage.googleapis.com/index.html