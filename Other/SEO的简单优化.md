---
title: SEO的简单优化
tags:
  - SEO
  - 网站优化
categories: SEO
permalink: seo/simple-seo-optimization
---

# SEO优化

### SEO (Search Engine Optimization) (搜索引擎优化)

## 分类：
- 白帽SEO
- 黑帽SEO

## 白帽SEO
### 内容上的SEO：
- 网站标题、关键字、描述
- 网站内容优化
- `Robot.txt`文件
- 网站地图
- 增加外链接引用

### 作为前端工程师的优化方法：
- 网站结构布局优化
- 网页代码优化

# 前端工程师与SEO

- 网站结构布局优化
    - 扁平化结构
        - 控制首页链接数量
        - 扁平化的目录层次
        - 导航SEO优化
    - 面包屑导航
        - 让用户了解当前所处位置
        - 是用户可以了解网站组织形式
    - 不可忽略的细节
        - logo及导航条
    - 控制页面的大小

# 网页代码优化

- `<title>`标题
- `<meta keywords>`关键词
- `<meta description>`网页描述

## 语义化代码
- `<h1>~<h6>`标签多用于标题
- `<ul>`标签多用于无序列表
- `<ol>`标签多用于有序列表
- `<dl>`标签多用于定数据列表
- `<em>,<strong>`表示强调

## 标签的优化
- `<a>`标签
    - `rel="nofollow"` *是一个HTML标签的属性值。这个标签的意义是告诉搜索引擎"不要追踪此网页上的链接"或"不要追踪此特定链接。*
- `<h1>` 正文标题，搜索引擎喜欢h1标签。样式用css修改。
- `<p> and <br>` *`<p>`标签仅仅用于段落`<br>`仅仅用于文本标签的换行*
- `<caption>` *`表格标题用<caption>定义`*
- `<img>` *应加上`<alt>`*
- `<strong><em>与<b><i>`  *`<strong>`的权重高于`<em>`，`<strong>`、`<em>`更讨搜索引擎喜欢*

# 小贴士
- 重要内容`HTML`代码放在最前面。(主要代码优先读取)
- 重要内容不要用JS输出
- 尽量使用iframe框架
- 谨慎使用`display:none`;
- 不断精简代码
