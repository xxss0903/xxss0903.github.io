
--
layout: blog
title: '从头开始搭建 GitHub Pages 博客'
date: 2017-01-24 12:11:34
categories: blog
tags: code
image: '/images/default.jpg'
lead-text: '使用Github Pages和Jekyll搭建博客'
---
***本文记录从Github创建项目为Pages博客，然后通过Jekyll配置基础的模版，因为笔者也刚接触Jekyll进行配置模版，所以是以一个简单的模版，并没有做太多的修改。然后使用Jekyll怎么创建博客，在使用Jekyll的过程中发现好多文章光写了如何配置Jekyll却没有写如何添加新的博客，所以这里进行添加。对于Git命令这里就不再添加，简单的add commit push 网上很多***

# 创建Github仓库
- 创建Github项目
![image](http://oojt5x2dl.bkt.clouddn.com/%E5%88%9B%E5%BB%BA%E4%BB%93%E5%BA%93.png)
创建完仓库之后clone到本地，此后有两种方法添加jekyll模版，一种是直接使用jekyll创建，这样创建的博客内容非常简单：

```
~ $ gem install jekyll
~ $ jekyll new blog2
~ $ cd blog2
~ $ jekyll serve //开启本地博客服务器，浏览器输入http://127.0.0.1:4000 查看博客 
```

本地服务器上查看
![image](http://oojt5x2dl.bkt.clouddn.com/%E6%9F%A5%E7%9C%8B%E6%9C%AC%E5%9C%B0%E5%8D%9A%E5%AE%A2.png)


当前产生的文件夹内容push到github上面，之后再浏览器栏输入 https://你的用户名.github.io就会显示当前你博客的内容了

# 配置Jekyll模版
到jekyll模版网站 http://jekyllthemes.org/ 查看下载
![image](http://oojt5x2dl.bkt.clouddn.com/jekyll%E6%A8%A1%E7%89%88%E7%BD%91%E7%AB%99.png)
选择一个模版，点击下载或者去它的github主页clone到本地
![image](http://oojt5x2dl.bkt.clouddn.com/%E4%B8%8B%E8%BD%BDjekyll%E6%A8%A1%E7%89%88.png)
下载模板到本地之后，要对模版中的内容进行更改，修改 ./_site/index.html 主页的配置，将一些别人的引用更改为自己的用户名，相关配置可以搜索 jekyll 配置
```
# Welcome to Jekyll!
#
# This config file is meant for settings that affect your whole blog, values
# which you are expected to set up once and rarely edit after that. If you find
# yourself editing this file very often, consider using Jekyll's data files
# feature for the data you need to update frequently.
#
# For technical reasons, this file is *NOT* reloaded automatically when you use
# 'bundle exec jekyll serve'. If you change this file, please restart the server process.

# Site settings
# These are used to personalize your new site. If you look in the HTML files,
# you will see them accessed via {{ site.title }}, {{ site.email }}, and so on.
# You can create any custom variable you would like, and they will be accessible
# in the templates via {{ site.myvariable }}.
title: Your awesome title
email: your-email@domain.com
description: > # this means to ignore newlines until "baseurl:"
  Write an awesome description for your new site here. You can edit this
  line in _config.yml. It will appear in your document head meta (for
  Google search results) and in your feed.xml site description.
baseurl: "" # the subpath of your site, e.g. /blog
url: "" # the base hostname & protocol for your site, e.g. http://example.com
twitter_username: jekyllrb
github_username:  jekyll

# Build settings
markdown: kramdown
theme: minima
gems:
  - jekyll-feed
exclude:
  - Gemfile
  - Gemfile.lock

```

将信息更改之后输入，当前文件夹输入命令，查看博客更改之后的样式是否满意，满意之后使用git命令，将当前博客内容上传到github，github会自行build，可能刚上传之后不会立即更改，等待一段时间之后会给你的邮箱发送通知告诉你pages已经编译了，此时查看你的博客地址就能查看
```
$ jekyll serve
$ git add .
$ git commit -m "更改模版设置"
$ git push origin master
```

# 使用Jekyll添加博客
使用jekyll命令添加博客非常简单，但是一定要注意博客的文件名的格式以及文件内头信息的内容进行更改。在 ./_posts/ 文件夹下面添加一个 ***.md 文件就行，注意文件名为 2017-04-17-myblog.md 这种格式，前面是日期，后面的名字不是你博客显示的标题，不要误解了，博客的标题会在md正文进行配置.
md文件内的头信息
```
---
layout: post
title:  "Welcome to Jekyll!"
date:   2017-04-17 18:40:46 +0800
categories: jekyll update
---
```

* 上面是刚创建的一个 markdown 文件的头，这个头注意保持这种样式，然后更改title，日期，分类，这几个会在博客编译之后进行，解析

从GitHub上面创建博客的内容大概就是这样了，之后就是通过markdown编辑你的博客了



