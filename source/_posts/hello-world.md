---
title: Welcome
author: Petalzu
categories: [笔记]
tags: [Hexo]
excerpt: "
欢迎来到兰州大学开源社区网站。
"
---

欢迎来到兰州大学开源社区网站。

关于本站主题：[Icarus](https://ppoffice.github.io/hexo-theme-icarus/)

关于本站框架：[Hexo](https://hexo.io/zh-cn/)



## 在GitHub Pages 上部署 Icarus
### 本地部署
在本地建立hexo文件夹，安装以下应用程序：  

Node.js (Node.js 版本需不低于 10.13，建议使用 Node.js 12.0 及以上版本)  
Git  

执行如下命令：
```bash
$ npm install hexo
```
添加Hexo 所在的目录下的 node_modules 添加到环境变量之中  

在想要建立hexo的 <folder>下执行如下命令：
```bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```

在 Hexo 根目录执行以下命令安装主题，有两种方式：
```bash
npm npm install -S hexo-theme-icarus hexo-renderer-inferno
```

在 Hexo 根目录的 _config.yml 文件中，将 theme 值修改为 Icarus：
```_config.yml
theme: Icarus
```
或执行命令：
```bash
$ hexo config theme icarus
```

添加新页：
```bash
$ hexo new page
```

启动hexo服务器，访问 localhost:4000 查看效果：
```bash
$ hexo server
```

### 推送
建立存储库，名为 <你的 GitHub 用户名>.github.io

将 main 分支 push 到 GitHub仓库：
```bash
$ git push -u origin main
```
或者使用GitHub Desktop等工具进行推送。

使用 node --version 指令检查你电脑上的 Node.js 版本，并记下该版本 (例如：v18.16.0)  
在储存库中前往 Settings > Pages > Source，并将 Source 改为 GitHub Actions。  
在储存库中建立 .github/workflows/pages.yml，并填入以下内容：  
```.github/workflows/pages.yml
name: Pages

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18.16.0'
      - uses: actions/cache@v3
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - run: npm install
      - run: npm run build
      - uses: actions/upload-pages-artifact@v2
        with:
          path: ./public
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

检查 https://<你的 GitHub 用户名>.github.io 是否已经部署成功。

### RSS
在 Hexo 根目录执行以下命令安装插件：
```bash
$ npm install hexo-generator-feed --save
```

在 Hexo 根目录的 _config.yml 文件中，添加以下内容：
```_config.yml
feed:
  type: rss2
  path: sitemap.xml
  limit: 20
  hub:
  content: false
```

在 _config.icarus.yml 文件中，找到RSS配置并修改为以下形式：
```_config.icarus.yml
RSS:
    icon: fas fa-rss
    url: /sitemap.xml
```

## 参考文档
[在 GitHub Pages 上部署 Hexo](https://hexo.io/zh-cn/docs/github-pages)  
[Getting Started with Icarus](https://ppoffice.github.io/hexo-theme-icarus/uncategorized/getting-started-with-icarus/)

