---
title: 使用hexo+next
date: 2019-08-01 22:17:52
categories: ['hexo','next']
tags: ['hexo','next']
---
## hexo+nex
[hexo官网文档](https://hexo.io/zh-cn/docs/)
[next文档](http://theme-next.iissnan.com/getting-started.html)

### 使用建议
  * 将hexo博客目录下的source上传到git新建工程，方便保存原始博客内容
### 碰见问题
在执行 hexo deploy 后,出现 error deployer not found:git
默认情况下，在哪个文件夹下运行npm，npm就在当前目录创建一个文件夹node_modules,
然后将要安装的程序安装到文件夹node_modules里面，所以必须在hexo目录下执行
`npm install hexo-deployer-git --save`
<!-- more -->
### 设置阅读全文按钮
在文章中使用\<\!--more--> 手动进行截断
### 引用资源图片，链接等
```
{% asset_path slug %}
{% asset_img slug [title] %}
{% asset_link slug [title] %}
```
### 引用文章
```
{% post_path slug %}
{% post_link slug [title] %}
```
{% link 官方指导 https://hexo.io/zh-cn/docs/tag-plugins#%E5%BC%95%E7%94%A8%E6%96%87%E7%AB%A0 %}
### 评论系统（gitment）
gitment，它是基于 github 开发的，是依靠于 GitHub Issues 的评论系统，Next 主题最新已经支持安装
评论显示在新建存放评论的仓库中的 issue 中
前提：更新 Next 主题（5.1.2 主题）
#### 注册OAuth application
在 github 中进行注册，进入 https://github.com/settings/profile，点击左侧 Developer settings，OAuth apps
得到Client ID 和Client Secret
#### 配置 next 主题文件
编辑主题配置文件：themes\next\ _config.yml，找到有关 gitment 的设置
必须配置github_user、github_repo、client_id、client_secret
#### 关闭某个页面的评论
在页面的 Front-matter 中添加 comments 字段，设为 false
Front-matter指的时页面文件开头的部分
## 参考资料
[手把手教你用Hexo+Github 搭建属于自己的博客](https://blog.csdn.net/gdutxiaoxu/article/details/53576018)
[Hexo使用攻略-添加分类及标签](https://linlif.github.io/2017/05/27/Hexo%E4%BD%BF%E7%94%A8%E6%94%BB%E7%95%A5-%E6%B7%BB%E5%8A%A0%E5%88%86%E7%B1%BB%E5%8F%8A%E6%A0%87%E7%AD%BE/)
[Hexo-设置阅读全文](https://www.jianshu.com/p/78c218f9d1e7)