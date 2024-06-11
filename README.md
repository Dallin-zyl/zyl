这是一个可以发布在GitHub的Jekyll博客，不需要编写代码或使用命令行即可获得一个部署在GitHub的博客。

## 更新内容

### V0.4.0

修订目录跳级会坏掉的问题，不算完美解决，但不会坏掉了。

增加对LaTeX渲染的支持，请见[这篇说明和示例](https://fromendworld.github.io/LOFFER/math-test/)。

增加置顶功能，只要在一个post的YAML Front Matter（就是文章头部的这段信息）中加入` pinned: true `，这篇文章就可以置顶了。

另外介绍一个给blog更换主题颜色的手法。blog用了一个开源的颜色表[Open Color](https://yeun.github.io/open-color/),该色表提供的可选颜色有：red, pink, grape, violet, indigo, blue, cyan, teal, green, lime, yellow。

blog的默认状态是teal，要更换主题颜色，只要打开文件` _sass/_variables.scss `，将文件中所有的teal全部替换成你想要的颜色。例如，查找teal，替换indigo，全部替换，commit，完成！


### V0.3.0

新版本增加目录功能，在post的信息中心加入` toc: true `，这篇博文就会显示目录了。

这次没有对config的修改，因此应该可以通过[这个方法](https://github.com/KirstieJane/STEMMRoleModels/wiki/Syncing-your-fork-to-the-original-repository-via-the-browser)，给自己提pull request来更新。

目录基于[jekyll-toc by allejo](https://github.com/allejo/jekyll-toc)制作。

目前试用发现了一点小问题：如果你的标题级数不按套路变化，它就会搞不懂…… 

` # 一级标题 `下面必须是` ## 二级标题 `，如果是` ### 三级标题 `它就人工智障了【手动扶额】

注意：目前目录仅在桌面版显示。


### 2019-06-30 V0.2.0

新版本进一步优化了一下样式，并且支持了基于GitHub Issues的评论Gitalk（请看下文的配置说明）。

如果你已经fork了LOFFER，想要更新到新版本的话，可以试试[这个方法](https://github.com/KirstieJane/STEMMRoleModels/wiki/Syncing-your-fork-to-the-original-repository-via-the-browser)，或者你也可以干脆删掉重来，只要保留自己的大部分config设定和所有的post就好。

## 支持的功能

使用Markdown文档在_post文件夹中发布博文，现有功能包括显示作者、置顶博文、添加目录。

博文YAML举例：

    ---
    layout: post
    title: Markdown语法简介
    date: 2013-07-16
    author: Shengbin 
    tags: [sample, markdown]
    comments: true
    toc: true	# 目录显示
    # pinned: true	#置顶blog
    ---

按照标签和日期查看博文归档。查看/tags 和/archive 页面。

链接博客主的社交媒体。在_config.yml中填写。

支持Disqus和Gitalk两种评论区。在_config.yml中设置。


## 致谢

* 谢谢



## 帮助这个项目

介绍更多人来使用它，摆脱lofter自由飞翔！

欢迎Issues和Pull Requests。

给我点一个☆吧！
