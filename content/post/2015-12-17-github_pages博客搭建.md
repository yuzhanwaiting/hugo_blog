---
title: github pages 博客搭建
date: 2015-12-17
categories: 
    - 其他
tags: 
    - 学习笔记
---

# 概述：

互联网行业，有许多技术人员有写博客的习惯。  
很多网站提供了该平台，比如博客园或者csdn等  
然而，这类网站风格过于单一，又不支持一些新的格式，例如`markdown`等，给实际操作带来了很大的不便  
如果自己搭建博客，则需要空间，域名等，二而这些也需要自己去维护。相比之下，更加麻烦  
今天，就给大家介绍一下`github`提供的博客功能--**github pages**

## 操作说明：

### 1 环境准备

* 系统： `Linux(ubuntu)`,`Mac OS`,`Windows`(不推荐使用)
* 软件： `ruby`, `rubygems`, `jekyll`
* 平台： `github`

### 2 项目创建

* 在`github`上创建新仓库，命名为: `username.github.io`
此处`username` 必须为`github`帐号名称,
命名不允许自定义，必须为 `username.github.io`

* 克隆项目到本地，并推送新信息

```
//克隆仓库到本地
~ $ git clone https://github.com/username/username.github.io
~ $ cd username.github.io

//新增首页文件
~ $ echo "Hello World" > index.html
~ $ git add --all
~ $ git commit -m "Initial commit"

//将首页文件推送至服务器
~ $ git push -u origin master
```

* 访问页面 `http://username.github.io` 则显示刚写入文件的信息


### 3 本地环境创建（以ubuntu为参考）

* 安装`ruby-install`，此用于安装ruby环境
```
~ $ wget -O ruby-install-0.5.0.tar.gz https://github.com/postmodern/ruby-install/archive/v0.5.0.tar.gz
~ $ tar -xzvf ruby-install-0.5.0.tar.gz
~ $ cd ruby-install-0.5.0/
~ $ sudo make install
```

* 接下来安装`ruby`
```
~ $ sudo ruby-install --system ruby
```

* 由于国内的`gem`源收到各种各样的限制，因此，需要更换`gem`源

```
~ $ gem sources --remove https://rubygems.org/
~ $ gem sources -a https://ruby.taobao.org/
~ $ gem sources -l
*** CURRENT SOURCES ***

https://ruby.taobao.org
```

* 安装`jekyll`，`jekyll`是用来生成静态网站的工具，`github pages`正是基于此构建
```
~ $ sudo gem install jekyll
```

* 安装`pygments`,由于`jekyll`语法着色依赖于，`python`库`pygments`，因此需要安装
```
~ $ sudo apt-get install python-pip
~ $ sudo pip install pygments
~ $ sudo gem install pygments.rb
```

* 安装`jekyll-paginate`，用于支持分页
```
~ $ sudo gem install jekyll-paginate
```

至此，环境搭建完成

### 4. jekyll简单介绍

#### 目录结构
```
.
├── _config.yml  
├── _drafts
|   ├── begin-with-the-crazy-ideas.textile
|   └── on-simplicity-in-technology.markdown
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
|   └── 2009-04-26-barcamp-boston-4-roundup.textile
├── _data
|   └── members.yml
├── _site
└── index.html
```

结构说明：

| 文件目录  |  描述  | 
| ------------ | -------- | 
| _config.yml  | 保存配置数据。 |
| _drafts      | drafts 未发布的文章。 |
| _includes   | 你可以加载这些包含部分到你的布局或者文章中以方便重用.这个标签{% include file.ext %} 来把文件 _includes/file.ext 包含进来。 |
| _layouts   | layouts 是包裹在文章外部的模板。布局可以在 YAML 头信息中根据不同文章进行选择。 这将在下一个部分进行介绍。标签 {{ content }} 可以将content插入页面中 |
| _posts   | 这里放的就是你的文章了 |
| _data   | 存放全站全局变量。格式为yml或yaml。如果存放的文件为member.yml，那么访问里面的内容就是site.data.member |
| _site   | 一旦 Jekyll 完成转换，就会将生成的页面放在这里（默认）。最好将这个目录放进你的 .gitignore 文件中。 |
| index.html   | 全局首页 |
| 其他文件   | 其他一些未被提及的目录和文件如  css 还有 images 文件夹， favicon.ico 等文件都将被完全拷贝到生成的 site 中 |


#### 配置说明
```
# Dependencies
highlighter:      pygments

encoding:         "utf-8"

# Setup
title:            '微端生活'            
tagline:          '郁湛的个人博客'
description:      '分享知识，记录知识！'
url:              http://hbphp.com

author:
  name:           'yuzhanwaiting'
  url:            https://github.com/yuzhanwaiting

paginate:         5
paginate_path:    "page:num"
gems:             [jekyll-paginate]


# Custom vars
version:          1.0.0
```

配置说明：

| 配置项 | 说明 | 
|:----------|:--------:| 
| highlighter  | 高亮组件，一般选择pygments |
| encoding     | 博客编码，一般贴写"utf-8" |
| title        | 博客标题                 |
| tagline      | 博客标语                 |
| description  | 博客描述 |
| url          | 博客地址 |
| paginate     | 博客每页页数 |
| paginate_path | 博客分页样式 |
| gems     | 依赖哪些库 |


#### 编写博客
关于`jekyll`具体的文档，可以查阅文档:[jekyll中文文档](http://jekyll.bootcss.com/)


* 按照`jekyll`规范建立目录文档等

* 编写`markdown`文档  
注意：  
1. 所有图片，静态文件等存放与根目录下资源文件夹（`assets`或`public`），加载路径为 `/assets/path/to/img`或`/public/path/to/img`
2. 区块代码标记修改为`highlight`标记

* 写好文档，直接推送至`github`，等几分钟后就可以正常访问

### 5. 配置域名
如果需要自定义域名,可以直接将域名作`CNAME`解析为 `username.github.io.`,注意后面有个`.`,几分钟后就可以用新的域名访问`github pages`博客了










