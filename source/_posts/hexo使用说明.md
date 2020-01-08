---
title: hexo使用说明
date: 2019-10-13 14:42:05
tags: 其它
thumbnail: '../assets/img/thumbnails/1.jpg'
---
## 简要流程
```bash
hexo server
hexo new <title>
hexo clean
hexo g
hexo d
```
<!-- more --> 

## 1.开启本地服务
```bash
hexo server
```

## 2.文章post
### layout种类和创建文章
```bash
hexo new [layout] <title>
```
layout可选，有3种， 默认为post，还可为page和draft。

## 3.草稿draft
### 1）创建草稿
```bash
hexo new draft <title>
```
### 2）发布草稿
_draft文件夹里是草稿文件，可以使用如下命令将draft转为post：
```bash
hexo publish <draftTitle>
```

## 4.模版Scaffold
Hexo 会根据 scaffolds 文件夹内相对应的文件来建立文件，例如：
```bash
hexo new photo "My Gallery"
```
即：Hexo 会尝试在 scaffolds 文件夹中寻找 photo.md，并根据其内容建立文章。

## 5.页面page
我目前还不知道该如何用中文称呼这类页面。我们可以把post和draft统称为blog pages，在这之外的一种就是normal pages， 类似一个网站上的“关于”，“了解我们”之类的页面。
### 创建page s
```bash
hexo new page c
```
和前两种不同，这个命令会在source文件夹内创建出c文件夹，与_posts，_drafts并列。文件夹里面有一个index.md文件。

刷新页面，你会发现c并没有出现在页面内，那它在哪儿呢？

在网址后面加上c/， 即http://localhost:4000/c/，就可以看到了。

正因为c不是一个blog page，所以它也不会出现在blog列表中，而是要通过URL去access。

## 6.修改默认设置
为什么一开始的时候我们用
```bash
hexo new <title>
```
就直接生成了post page呢？
因为默认的设置。

打开熟悉的_config.yml文件，找到
```bash
default_layout: post
```
这句表示默认的页面会新建成post格式的。

所以，如果你习惯先把文章写成草稿，那就把它改成draft就好。
```bash
default_layout: draft
```

## 7.部署到github
打开站点配置文件 _config.yml，翻到最后，修改为
YourgithubName就是你的GitHub账户
```
deploy:
  type: git
  repo: https://github.com/YourgithubName/YourgithubName.github.io.git
  branch: master
  ``` s
这个时候需要先安装deploy-git ，也就是部署的命令,这样你才能用命令部署到GitHub。
```bash
npm install hexo-deployer-git --save
``` 

然后
```bash
hexo clean
hexo generate
hexo deploy
``` 

其中 hexo clean清除了你之前生成的东西，也可以不加。
hexo generate 顾名思义，生成静态文章，可以用 hexo g缩写
hexo deploy 部署文章，可以用hexo d缩写

注意deploy时可能要你输入username和password。

得到下图就说明部署成功了，过一会儿就可以在http://yourname.github.io 这个网站看到你的博客了！！




