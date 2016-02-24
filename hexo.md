## hexo ##


**<p>hexo install:</p>**
<pre><code>##############
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server
</code></pre>

**你可以执行下列命令来创建一篇新文章:**
<pre><code>##############
hexo new [layout] <title>
</code></pre>

**布局（Layout）**
Hexo 有三种默认布局：post、page 和 draft，它们分别对应不同的路径，而您自定义的其他布局和 post 相同，都将储存到 source/_posts 文件夹。

布局	路径  
post	source/_posts  
page	source  
draft	source/_drafts  

**文章添加标题、tag和分类**  
可以在md文件手动加入categories:项

<pre><code>##############
title: git
date: 2016-01-24 12:49:26
categories:
tags:
---
</code></pre>
也可以打开scaffolds/post.md文件，在tages:上面加入categories:,保存后，重新执行hexo n 'name'  
在编辑文章的时候，tags:后面是设置标签的地方，如果有多个标签的话，可以用下面两种办法来设置：

<pre><code>##############
tages: [标签1,标签2,...标签n]
或者：
tages: 
- 标签1
- 标签2
...
- 标签n
</code></pre>
当有重复标签和分类的时候，删除db.json即可生成
