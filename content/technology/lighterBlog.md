+++
title = "极简博客"
date = 2021-06-28T20:30:53+08:00
draft = false
indent = true
initial = false
toc = false
+++

作为一个半吊子程序员，一个向程序员团体靠拢的“死忠粉”。**博客**几乎成了标配。从最开始了解到的 [WordPress](notion://www.notion.so/wordpress.org) 和使用过一段时间的 [Typecho](notion://www.notion.so/typecho.org) 等，再到后来入坑 [Github Page](notion://www.notion.so/github.com)  ，尝试 [Hexo](notion://www.notion.so/hexo.io) 以及 [Hugo](notion://www.notion.so/gohugo.io) 等静态网页生成器。兜兜转转也已有两年。中间也断过些时日，前些日子突然看到 [谢益辉](notion://www.notion.so/yihui.org) 的个人博客，简洁明快的博客主页让我重燃对个人博客的追求。于是乎便诞生了自己建一个极简博客的想法。为自己的日常文字记录开辟一块安静的“**网内之地**”。

根据本人半吊子的前端设计水平和烂泥一般的逻辑思维与学习能力，我对博客的要求并不高：

- 能够呈现类似书籍的排版
- 能够直接使用浏览器进行打印输出
- 够**快**、够**方便**

基于上面的基本要求，我果断选择了 [Hugo](notion://www.notion.so/gohugo.io) 作为自己的网页生成器，并且购买了 [阿里云](https://cn.aliyun.com/) 的学生机作为服务器，通过 [阿里云](https://cn.aliyun.com/) 做了备案—— [Shengqiang.top](https://shengqiang.top/) 这个域名，（ 这里不得不提一句！[阿里云](notion://www.notion.so/cn.aliyun.com) 的备案确实方便，周六开始备案，周一便通过管局审核，成功备案。相较于之前十几天的备案经历，这次让我体验到了无与伦比的顺畅和舒适。可能又运气成分吧，天知道！）购买 [阿里云OSS对象存储](https://www.aliyun.com/product/oss?spm=5176.19720258.J_8058803260.100.54212c4a4K7lf4)   作为自己的图床。

不过在选择 [Hugo](notion://www.notion.so/gohugo.io) 主题时我犯了难，我其实想用 [谢益辉](notion://www.notion.so/yihui.org) 大佬的博客主题，可是我追求的极简是不想为博客增加类似菜单栏一类的部件。而其他 [Hugo Theme](https://themes.gohugo.io/) 对我而言又太繁杂。我不想要一个功能齐全的网站，我只想要一个能够随时随地查看自己笔记和日志的博客。但是我喜欢  [谢益辉](notion://www.notion.so/yihui.org) 大佬的排版布局。于是我半抄半改，制作了符合自己习惯的 [Hugo 主题](notion://www.notion.so/github.com/lulyrose/hugo-note) 。当然，这个主题漏洞很多，代码也不规范。可暂时能用，后续我会慢慢重构和修改的。

博客使用的字体是通过 [Google Fonts](https://fonts.google.com/) 的 [思源宋体](https://source.typekit.com/source-han-serif/cn/) 。而排版的话遵循自己的意愿，采用 首行缩进 无段距 2倍行距 的中文排版样式。主旨便是简洁大方。最终效果也不错。

在打印输出方面，使用如下的 `CSS` 代码，自定义打印样式，确保打印输出的正确性。

```css
@media print {
	footer {
		display: none;
	}
	.post {
		padding: 0;
		box-shadow: none;
	}
	.post-nav {
		display: none;
	}
	.comments {
		display: none;
	}
	pre {
		white-space: pre-wrap;
		word-wrap: break-word;
	}


@page {
	size: A4 protrait;
	margin: 2.54cm 3.17cm 2.54cm 3.17cm;
}

```

博客的 [Hugo 源文件](notion://www.notion.so/github.com/lulyrose/shengqiang.me) 托管在 [Github](notion://www.notion.so/github.com) 上，并同步部署到服务器。国内访问速度较快，编写博客方便，迁移和保存也较为健壮。
