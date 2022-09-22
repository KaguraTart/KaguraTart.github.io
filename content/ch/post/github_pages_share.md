---
date: 2022-08-20T17:00:10+08:00
description: "本文使用hugo的ananke主题在Github pages上使用workflow action自动生成和部署博客"
featured_image: "/images/miku/miku1.jpg"
tags: ["hugo","github","博客"]
title: "使用hugo在Github上自动部署博客"
---

## 本文概要：
- hugo安装和部署
- GitHub的repository创建
- GitHub的action创建


## 0x00 Linux(ubuntu)下hugo的下载和安装

使用apt进行二进制安装

```shell
$ sudo apt-get hugo
```

## 0x01 使用hugo生成站点

使用Hugo快速生成站点

```shell 
$ hugo new site <your project filename>
```

举例子：比如希望生成到 /myweb/tart 路径文件夹：

```shell
$ hugo new site /myweb/tart
```

## 0x02 创建文章

### 方法1

使用hugo命令创建：

```shell
$ hugo new about.md
```
<code>about.md</code> 自动生成到了 <code>content/about.md</code> ，打开 <code>about.md</code> 看下：
```
---
date = "2015-10-25T08:36:54-07:00"
draft = true
title = "about"
---
正文内容
```

## 0x03 安装主题

到![主题](https://themes.gohugo.io/) 挑选一个心仪的皮肤，比如你觉得 Hyde 皮肤不错，找到相关的 GitHub 地址，创建目录 themes，在 themes 目录里把皮肤 git clone 下来：
```shell
# 创建 themes 目录
$ cd quickstart
$ git init
$ git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```
## 0x04 运行hugo

使用命令行运行hugo并部署

```shell
$ hugo server -D

                   | ZH | EN  
-------------------+----+-----
  Pages            | 24 | 15  
  Paginator pages  |  2 |  0  
  Non-page files   |  0 |  0  
  Static files     | 14 | 14  
  Processed images |  0 |  0  
  Aliases          |  3 |  1  
  Sitemaps         |  2 |  1  
  Cleaned          |  0 |  0  

Built in 168 ms
Watching for changes in /media/tart/B224064024060855/linux/web/tart_web/{archetypes,content,data,layouts,static,themes}
Watching for config changes in /media/tart/B224064024060855/linux/web/tart_web/config.toml, /media/tart/B224064024060855/linux/web/tart_web/themes/ananke/config.yaml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
```

## 0x05 Github仓库创建和本地clone

### 仓库创建
在GitHub上创建一个新的repo，名字 username.github.io 

![github1](/images/github_pages_share/github1.png)

### 本地clone

```shell
$ git clone https://github.com/username/username.github.io
```

### 初始化和提交

```shell
$ cd username.github.io
$ echo "Hello World" > index.html
$ git add --all
$ git commit -m "Initial commit"
$ git push -u origin main
```


## Github action 自动化部署

创建 <code>.github/workflows</code>，并且创建gh-pages.yml文件。其中内容如下：

```yml

name: GitHub Pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --minify --enableGitInfo
        
      - name: Upload artifact
        if: github.ref == 'refs/heads/main'
        uses: actions/upload-pages-artifact@v1
        with:
          path: public
          
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        # uses: actions/deploy-pages@v1
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
  deploy-tart:
    needs: deploy
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-20.04
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v1
```
则可以完成自动化部署（如下图）。
![自动化部署](/images/github_pages_share/git2.png)


