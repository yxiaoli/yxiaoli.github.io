+++
title =  "Blog Tool"
date =  2015-12-27T13:26:32+02:00
tags = ["techniques"]
categories = ["tech"]
description = ""
# 此处可以写摘要！
menu = ""
banner = "banners/blog-tool.jpg"
+++

# Excerpt

> For most blog lovers, they usually experience these three phases.
First step, as beginners we feel curious and excited to try all kinds way, where we can post our articles. Writing on a free platform,though the templates or layout looks monotonous,still it is a good choice. Later you may have more requirements which the platform can no longer fulfil, then you choose to buy the domain and setup your own website. In the third step, you're tired with maintain a website and then let someone's help you, thus you only focus on the writing.

搭建自己的博客是有点折腾，但是会乐在其中。 考虑到技术文章会涉及到大量的英文单词，或者命令就用英文写。 概述类和心得篇就用中文。 p.s.[旧csdn博客地址](http://blog.csdn.net/u011613321)

所以针对新博客搭建有几个plan：

- 初期：基本功能分类， 标签，联系方式
- 中期：评论， 网站分析， rss， 图片托管，Gallery
- 长期：和顶级域名分离，blog分En 和Ch， 兴许还有De。
初期已经差不多完成了，继续折腾。

# Disqus 添加评论功能

在很久以前评论还是属于博客主的资源，在搬迁的过程中文章评论一个不能丢。数据存在网站数据中。 近年网络社交兴起博客主为了吸引访客互动会使用第三方网站托管评论。Disqus就是这样的第三方社会化评论平台，注册Disqus后你会发现：这根本就是个社交平台啊！ 所以同志们在你的博客上评论后，你登陆Disqus 后台，会看见评论， 推荐，喜爱 等等。

## 个人信息

1. Profile: Full Name(昵称，任意取)
2. Avatar: 头像支持本地上传，网络图像URL，所在网站的默认头像，Facebook头像，Twitter头像。当然，后两者需要绑定对应的社交帐号。
3. Serives: 第三方帐号的管理界面，可进行绑定、解绑社交帐号，目前所支持的除了三大社交网络帐号，还有Yahoo! 已绑定的帐号名在Disqus评论系统上是公开的。

## 账户

1. Username ： 唯一
2. Email ： 邮箱进行验证。

# 开始加入Disqus

1. 注册
2. 这里注册的Site name 就是我们将要用到的shortname
3. 注册后你会得到一个 sherrysblog.disqus.com/东西，其中sherrysblog就是我的shortname, 然后填到 config.toml里。
4. 在我的账户内https://sherrysblog.disqus.com/admin/settings/general/ 可以进行个人网站的设置， 语言设置等。
   在setting里面的信息和config.toml要一致。

## 缺点

- 有收费的可能
- 不支持国内社交账号
- 没有私信功能。
- 可能被墙

# Google Analytics 网站分析

google analytics
这其实是个很强大的工具，但是想想挺可怕的。因为如果作为商业用户，会实时监控我们大众用户的行为，这就是所谓大数据%>_<%

1. 注册 google analytics的账号
2. 填写自己blog 的位置tintinsnowy.com
3. 将Tracing code 粘贴到自己config.toml中。
4. 通过google analytics 后台账号查看用户访问情况。
—–TBC

writen by Sherry
photographed by Sherry. 'Wiener Staatsoper'
