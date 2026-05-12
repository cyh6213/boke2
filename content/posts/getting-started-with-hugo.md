---
title: "Hugo 入门指南：快速搭建个人博客"
date: 2026-05-13T10:00:00+08:00
draft: false
tags:
  - Hugo
  - 博客
  - 静态站点
categories:
  - 技术
---

# Hugo 入门指南：快速搭建个人博客

## 什么是 Hugo

Hugo 是一个用 Go 语言编写的静态站点生成器，以其极快的构建速度和丰富的功能著称。

## 为什么选择 Hugo

### 1. 构建速度极快

```bash
# 构建整个站点只需不到1秒
hugo --minify
```

### 2. 零依赖

只需一个二进制文件，无需 Node.js、Python 等运行时环境。

### 3. 丰富的主题生态

Hugo 拥有大量精美的主题，如 PaperMod、Blowfish 等。

## 快速开始

### 安装 Hugo

```bash
# macOS
brew install hugo

# Windows
scoop install hugo-extended

# Linux
sudo apt install hugo
```

### 创建站点

```bash
hugo new site myblog
cd myblog
git init
```

### 安装主题

```bash
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

### 创建文章

```bash
hugo new content posts/my-first-post.md
```

### 本地预览

```bash
hugo server -D
```

## 部署到 GitHub Pages

创建 `.github/workflows/deploy.yml`：

```yaml
name: Deploy Hugo to GitHub Pages

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: true
      - uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
      - run: hugo --minify
      - uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

## 总结

Hugo 是一款优秀的静态站点生成器，适合快速搭建博客、文档站点等。它的速度、简洁性和丰富的生态使其成为很多开发者的首选。

如果你也想搭建自己的博客，不妨试试 Hugo！
