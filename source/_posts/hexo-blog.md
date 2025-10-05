---
title: 新建一个博客以及博客维护
date: Invalid date
tags:
  - blog
  - 教程
excerpt: 这篇文档记录了如何从零开始新建一个博客，以及为后续维护提供一个指导
author: HeWenXuan
language: zh-CN
cover: https://youke1.picui.cn/s1/2025/10/05/68e233fdb72bc.png
license: CC BY-NC-SA 4.0
---


# 简述
本文档主要记录了如何从零开始新建一个博客，以及为后续维护提供一个指导。该博客采用[Hexo](https://hexo.io/zh-cn/)框架，部署到github pages，主题则为[redefine](https://redefine-docs.ohevan.com/zh/getting-started)

# 环境
- Windows 11或Ubuntu 24.04

# 建立第一个博客
**建议直接查看官方教程，会比本文档更加全面**
- [Hexo官方文档](https://hexo.io/zh-cn/docs/)
- [redefine主题官方文档](https://redefine-docs.ohevan.com/zh/getting-started)
## 1. 下载并安装git
- Windows：请前往[git官网](https://git-scm.com/)下载安装包
- Ubuntu：在终端执行以下命令
    ```bash
    sudo apt update #更新软件包列表
    sudo apt-get install git-core -y #安装git
    ```
输入以下命令，如果有版本号输出则表示git安装成功
```bash
git --version
```
## 2. 下载并安装Node.js
- Windows：请前往[Node.js官网](https://nodejs.org/zh-cn)下载安装包并安装。不过更建议使用NVM来管理Node.js版本，此处不过多介绍nvm的安装和使用，建议自行搜索相关教程
- Ubuntu：使用如下命令安装。但是笔者首次建站是在windows上操作的，建议自行查找相关更权威的文档
    ```bash
    sudo apt update #更新软件包列表
    sudo apt install -y curl #安装curl
    curl -fsSL https://deb.nodesource.com/setup_23.x -o nodesource_setup.sh #下载Node.js安装脚本
    sudo -E bash nodesource_setup.sh #运行安装脚本
    sudo apt install -y nodejs #安装Node.js
    ```
输入以下命令，如果有版本号输出则表示node.js安装成功
```bash
node -v
```
## 3. 安装Hexo
在终端执行以下命令安装Hexo
```bash
npm install -g hexo-cli
```
输入以下命令，如果有一堆版本号输出则表示Hexo安装成功
```bash
hexo -v
```
## 4. 创建博客
在你想要存放博客的目录下打开终端，执行以下命令
```bash
hexo init <folder>
cd <folder>
npm install
```
其中`<folder>`为你想要创建的博客目录名称,完成后项目文件夹将如下所示
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
具体各级文件夹分别是什么含义请自行查看官方文档，这里不需赘述
## 5. 安装并启用redefine主题
在终端执行以下命令
```bash
npm install hexo-theme-redefine@latest
```
然后打开`_config.yml`文件，找到`theme`字段，将其值改为`redefine`
```_conig.yml
theme: redefine
```
## 6. 配置redefine主题
创建一个`_config.redefine.yml`文件(请确保文件名完全一致)，并将[此处](https://github.com/EvanNotFound/hexo-theme-redefine/blob/main/_config.yml)内容复制进去,此时redefine主题会自动覆盖hexo默认主题的配置项。然后具体怎么按自己喜欢的样式配置请查看[官方文档](https://redefine-docs.ohevan.com/zh/basic)，内容过多此处不再赘述。
## 7. 本地预览
在博客路径下的终端执行以下命令
```bash
hexo clean #清除缓存（可选）
hexo g #生成静态文件
hexo s #启动本地服务器
```
此时在浏览器访问`http://localhost:4000`即可预览博客
## 8. 创建git仓库
创建一个GitHub仓库，且仓库名字必须是`<github_username>.github.io`，其中`<github_username>`为你的GitHub用户名，勾选`Initialize this repository with a README`，然后点击`Create repository`按钮创建仓库。
## 9. 部署到GitHub Pages
在博客路径下的终端执行以下命令
```bash
npm install hexo-deployer-git --save #安装部署插件
```
然后打开`_config.yml`文件，找到`deploy`字段，修改为如下内容
```_config.yml
deploy:
  type: git
  repository: git@github.com:用户名/用户名.github.io.git # 替换为你的仓库地址
  branch: main #如果你的默认分支是master则改为master
```
然后执行以下命令部署
```bash
hexo clean #清除缓存（可选）
hexo g #生成静态文件
hexo d #部署到GitHub Pages
```
此时访问`https://<github_username>.github.io`即可看到你的博客上线了

# 如何写文章
本教程是以草稿-正文的形式教学，即先创建草稿，草稿写完后发布为正式文章。但是实际使用过程中也可以直接新建正文具体操作为把第1步中的`draft`改为`post`即可。
## 1. 创建一个草稿
在博客路径下的终端执行以下命令
```bash
hexo new draft "文章标题"
```
此时你的`source/_drafts`目录下会生成一个新的markdown文件，文件名为`文章标题.md`，打开该文件即可编辑文章内容  
如果要预览草稿则输入以下命令
```bash
hexo clean #清除缓存（可选）
hexo g #生成静态文件
hexo server --draft #启动本地服务器（可查看草稿渲染）
```
## 2. 发布文章
在博客路径下的终端执行以下命令
```bash
hexo publish draft "文章标题"
```
此时你的文件会从`source/_drafts`目录移动到`source/_posts`目录下
## 3. 推送到GitHub Pages
在博客路径下的终端执行以下命令
```bash
hexo clean #清除缓存（可选）
hexo g #生成静态文件
hexo d #部署到GitHub Pages
```

# 跨平台管理博客
如果你想在多台电脑或者不同的操作系统上管理你的博客，可以使用git来同步博客内容。以下是具体操作步骤
## 1. 推送本地博客重要配置到远程仓库
可以直接使用相同仓库用不同的分支做区分即可，在博客路径下的终端执行以下命令
```bash
git init #初始化git仓库
git add . #添加所有文件
git commit -m "Initial commit" #提交更改
git remote add origin <url> #连接远程仓库
git checkout -b <branch> #新建并切换分支
git push -u origin <branch> #推送到远程仓库
```
## 2. 在另一台电脑上克隆仓库
在另一台电脑上配置好博客需要的环境，然后在你想要存放博客的目录下打开终端，执行以下命令
```bash
git clone -b <branch> <url> #克隆远程仓库的指定分支
cd <folder> #进入博客目录
npm install #安装依赖
```
## 3. 修改博客内容
写完后相同步骤生成并部署
```bash
hexo clean #清除缓存（可选）
hexo g #生成静态文件
hexo d #部署到GitHub Pages
```
## 4. 然后将更改推送到远程仓库
```bash
git add . #添加所有文件
git commit -m "Update blog" #提交更改
git push origin <branch> #推送到远程仓库
```
# 5. 更新博客环境
如果你在一台电脑上修改了博客内容，那么另一台电脑需要先拉取最新的更改，然后再进行修改
```bash
git pull origin <branch> #拉取远程仓库的最新更改
```

# 一些我在使用过程中遇到的问题
## 1. Redefine主题的`nodejieba`安装失败
### 原因：
我在把window建立的博客克隆到Ubuntu上时，没有做好隔离，导致Windows编译的文件：nodejieba.node被Ubuntu编译器编译出来的强制覆盖了，导致无法使用，且在windows上删除`node_modules`文件夹，然后重新执行`npm install`，重新编译安装所有依赖也无法使用。可能还有我未知的原因还未查明。
### 解决方法：
使用如下命令创建模拟的`nodejieba`模块
```bash
# 1. 先删除有问题的文件(直接删除文件夹也可以)
rm -rf node_modules/nodejieba
# 2. 重新创建目录
mkdir -p node_modules/nodejieba
# 3. 使用 PowerShell 正确创建文件（推荐）
@"
module.exports = {
  cut: function(text) { return text ? [text] : []; },
  cutAll: function(text) { return text ? [text] : []; },
  cutForSearch: function(text) { return text ? [text] : []; },
  tag: function(text) { return text ? [[text, 'n']] : []; },
  extract: function(text, topn) { return text ? [{word: text, weight: 1}] : []; },
  load: function() { return true; },
  insertWord: function(word) { return true; }
}
"@ | Out-File -FilePath node_modules/nodejieba/index.js -Encoding utf8
# 4. 测试
hexo g
hexo s
```

# todo
- [ ] 更换域名
- [ ] 让国内访问更快
- [ ] 添加评论系统
- [ ] 添加搜索功能
- [ ] 添加Tag功能
- [ ] 添加友链