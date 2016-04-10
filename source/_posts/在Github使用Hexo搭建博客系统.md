---
title: 在Github使用Hexo搭建博客系统
date: 2016-04-01 21:43:41
tags: [编程,Git,Github]
---

# 一、配置Github环境

## 安装Git

## 配置ssh-key

1. 检查ssh-key的设置

 ```
# 第一次安装时没有该目录
$ cd ~/.ssh 
```

<!--more-->

2. 生成新ssh-key

 ```
# rsa算法，C后面接邮箱账号；表示根据邮箱生成key
$ ssh-keygen -t rsa -C "example@example.com"
```
3. 添加ssh-key到Github

 登陆Github-->Account Settings--->SSH Public keys ---> add another public keys

4. 测试

 ```
$ ssh -T git@github.com
```

5. 设置用户信息

 ```
## 当电脑只需用到一个Github账号时，可以使用全局的用户信息
$ git config --global user.name "name"//用户名
$ git config --global user.email "example@example.com"//邮箱
```

# 二、当电脑需要多个Github账号切换时

1. 配置第二个账号的ssh-key

 ```
$ssh-keygen -t rsa -C "example@example.com"
$ssh-add ~/.ssh/id_rsa_second
```

2. 配置ssh-key到github
3. 修改~/.ssh/config文件

 ```
#默认的github
Host github.com
  HostName github.com
  IdentityFile ~/.ssh/id_rsa
#第二个github
Host github_second
  HostName github.com
  IdentityFile ~/.ssh/id_rsa_second
```

4. 使用别名pull/push代码

 ```
 git clone git@github_second:username/reponame
```

5. 取消global用户信息

 ```
git config --global --unset user.name
git config --global --unset user.email
```

6. 每次commit前都要执行下面代码

 ```
git config  user.email "example@example.com"
git config  user.name "name"
```

# 三、生成<github_name>.github.io博客

1. 登录github，创建一个repository，命名为<github_name>.github.io
2. 进入项目，点击Settings，在Github Pages部分点击Launch automatic page generator，下一步，下一步就可以了。
3. 通过http://<github_name>.github.io访问你的静态站点

# 四、使用git提交更新

>Tip：当对git命令不熟悉时，可以通过`$git xxx --help`查看帮助文档，注意xxx代表不熟悉的命令，比如clone。也可以查看[廖雪峰的Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)和[Git官网](https://git-scm.com/book/zh/v2)学习。

1. 克隆远程仓库

 ```
# 电脑中只有一个github账号时
git clone https://github.com/username/reponame
# 电脑中有两个个github账号时，github_second表示第二个账号的host
git clone git@github_second:username/reponame
```

2. 移除所有文件

 ```
# 进入仓库文件夹（工作区）
$cd reponame
# 强制移除所有文件
$git rm -rf .
```

3. 添加更新到仓库暂存区中

 可以复制要添加的文件到仓库文件夹（工作区）中，然后执行命令
`$git add *`

4. 提交更新到本地仓库

 ```
$git commit -a -m "注释"
```

5. 提交更新到远程服务器

 ```
# origin表示源仓库
$git push origin master
```

# 五、分支管理

## 新建分支

```
# 切换到分支newbranch，-b表示没有就创建（branch/build）
$git checkout -b newbranch
# 创建文件hello
$touch hello
# 添加到暂存区
$git add hello
# 提交到本地仓库
$git commit -m "add a file hello"
# 提交到远程仓库的newbranch分支，-u表示set-upstream
$git push -u origin newbranch
```

## 设置默认分支

在项目的Settings-->Branches下可以修改Default branch
修改默认分支，其实就是把版本库的头指针HEAD
指向了其他分支，通过`$git branch -r`（r表示remotes）可以查看所有远程分支和默认分支。

## 合并分支

```
$git merge newbranch
```

## 切换分支

```
# 切换到master分支工作
$git checkout master
```

## 删除分支

Git在删除分支时为避免数据丢失，默认禁止删除尚未合并的分支。所以，如果分支尚未合并，使用`$git branch -d newbranch`会报错。
使用`$git branch -D newbranch`可以强制删除分支。

## 删除远程分支

```
$git push origin --delete testbranch
```

# 六、使用github协同开发

## 开发者fork源仓库

每个开发者都从源仓库中Fork代码，然后独立开发。完了，在pull request合并到源仓库中。

## 把开发者仓库clone到本地

```
# 电脑中只有一个github账号时
git clone https://github.com/username/reponame
# 电脑中有两个个github账号时，github_second表示第二个账号的host
git clone git@github_second:username/reponame
```

## 构建功能分支进行开发

```
>>> git checkout develop
# 切换到`develop`分支

>>> git checkout -b feature-discuss
# 分出一个功能性分支

>> touch discuss.js
# 假装discuss.js就是我们要开发的功能

>> git add .
>> git commit -m 'finish discuss feature'
# 提交更改

>>> git checkout develop
# 回到develop分支

>>> git merge --no-ff feature-discuss
# 把做好的功能合并到develop中

>>> git branch -d feature-discuss
# 删除功能性分支

>>> git push origin develop
# 把develop提交到自己的远程仓库中
```

## pull request

点击绿色按钮pull request，将自己仓库的分支合并到源分支中

## 管理员测试、合并

1. review代码

```
>> git checkout develop
# 进入他本地的develop分支

>> git checkout -b khhhshhh-develop
# 从develop分支中分出一个叫khhhshhh-develop的测试分支测试我的代码

>> git pull https://github.com/khhhshhh/practice.git develop
# 把我的代码pull到测试分支中，进行测试
```
2. 合并分支

```
>> git checkout develop
>> git merge --no-ff khhhshhh-develop
>> git push origin develop
```

# 七、安装和配置Hexo

## 需要环境
  - Node.js
  - Git
## 安装

```
npm install hexo-cli -g
npm install hexo --save

#如果命令无法运行，可以尝试更换taobao的npm源
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

## 基本配置

```
#Hexo 将会在指定文件夹中新建所需要的文件。
$ hexo init <folder>
$ cd <folder>
$ npm install
```

```
# 安装Hexo插件
npm install hexo-generator-index --save
npm install hexo-generator-archive --save
npm install hexo-generator-category --save
npm install hexo-generator-tag --save
npm install hexo-server --save
npm install hexo-deployer-git --save
npm install hexo-deployer-heroku --save
npm install hexo-deployer-rsync --save
npm install hexo-deployer-openshift --save
npm install hexo-renderer-marked@0.2 --save
npm install hexo-renderer-stylus@0.2 --save
npm install hexo-generator-feed@1 --save
npm install hexo-generator-sitemap@1 --save
```

```
# 在本地查看效果
$hexo server
```

## 主题配置

1. 修改根目录的_config.yml

```
# Hexo Configuration
## Docs: http://hexo.io/docs/configuration.html
## Source: https://github.com/tommy351/hexo/

# Site #整站的基本信息
title: IT协会 #网站标题
subtitle: 学习 总结 分享 #网站副标题
description: 学习 总结 分享 #网站描述
author:  itxiehui#网站作者
email: gdinit@163.com #联系邮箱
language: zh-CN

# URL
## If your site is put in a subdirectory
url: http://itxiehui.github.io #你的域名
root: /
permalink: :year/:month/:day/:title/
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code

......

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10 #每页10篇文章
pagination_dir: page

# Disqus #社会化评论disqus
disqus_shortname:

# Extensions
## Plugins: https://github.com/tommy351/hexo/wiki/Plugins
## Themes: https://github.com/tommy351/hexo/wiki/Themes
theme: jacman #修改主题
exclude_generator:
Plugins:
- hexo-generator-feed
- hexo-generator-sitemap

......

# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy: # 部署位置
  type: github
  repository: https://github.com/cnfeat/cnfeat.github.io.git
  branch: master     
```

## 写作

使用hexo new "newpost"创建一个新文件，然后使用hexo g -d生成并部署到github上。注意在_config.yml中配置的deploy的repository要看是否电脑有多个github账号。

```
#常用命令
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #将.deploy目录部署到GitHub
```

```
#简写
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

## 自定义404页面

在主题的source/文件夹下新建一个404.html，内容如下：

```
<!DOCTYPE HTML>
<html>
<head>
  <meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="robots" content="all" />
  <meta name="robots" content="index,follow"/>
  <title>IT协会知识库</title>
</head>
<body>

<script type="text/javascript" src="http://www.qq.com/404/search_children.js" charset="utf-8" homePageUrl="your site url " homePageName="回到我的主页"></script>

</body>
</html>
```

再次执行`$ hexo g -d`即可快速部署。

----------------

><span style="font-size:12px">本文标题: <a href="{{ permalink }}">{{ title }}</a>
文章作者: <a href="http://itxiehui.github.io/">IT协会知识库</a>  
许可协议: <img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" /><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">©署名-非商用-相同方式共享 4.0</a></span>