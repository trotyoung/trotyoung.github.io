---
layout: post
title:  "Github Page"
date:   2022-06-15 15:21:00 -0400
categories: "common"
---
使用Github Pages结合Jekyll能够比较简单的搭建一个博客系统。这不是一篇完整详细的指导文档，只是针对官网文档进行一些说明，提供一个大致思路。
# 准备工作
- github账号，以及两个仓库（github pages仓库和图床仓库）
- mac主机，Linux主机
- VS-Code或者Typora
- Upgit

# 测试环境
## 在Linux主机搭建测试环境
> 以Ubuntu为例，安装Jekyll
```
sudo apt-get install ruby-full build-essential zlib1g-dev
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

gem install jekyll bundler
```

如果没有[准备工作](#准备工作)中所提到的仓库，可以先在github网站根据[为站点创建仓库](https://docs.github.com/cn/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll#creating-a-repository-for-your-site)这篇文档操作，在github创建 <用户名>.github.io名称的github仓库。

[创建站点](https://docs.github.com/cn/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll#creating-your-site
)的过程，简单来说就是:
- 准备目录
``` bash
cd <用户名>.github.io
mkdir docs
cd docs
```
- 在docs目录创建新 Jekyll 站点
```
jekyll new --skip-bundle .
```
- 编辑Gemfile文件
``` bash
# 注释这行，（版本可能不同）
gem "jekyll", "~> 4.2.2"
# 解注释 # gem "github-pages" 开头的行（版本可能不同）
gem "github-pages", "~> 226", group: :jekyll_plugins
```
- 执行安装
```
bundle install
```
- 拉取github仓库至测试主机
```
sudo apt install git
git clone git@github.com:trotyoung/trotyoung.github.io.git
```
# 测试站点
[使用 Jekyll 在本地测试 GitHub Pages 站点](https://docs.github.com/cn/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)

# 本地编辑
## 编辑器选择
Visual Studio Code或者Typora

## 图片上传
- 准备github图床仓库，生成token, 采用最小化权限，只授权访问图床仓库，只给予必要的权限。
<img src="https://raw.githubusercontent.com/trotyoung/super-journey/main/2022/12/upgit_20221211_1670791924.png" alt="upgit_20221211_1670791924.png" style="zoom:50%;" />


- 本地安装upgit，注意不要安装到需要root权限的路径.
```
tree opt/upgitdir
opt/upgitdir
├── config.toml
├── history.log
├── upgit
└── upgit.log
```

- Visual Studio Code 安装`upgit-vscode-extension`插件，或者Typora配置自定义命令。

