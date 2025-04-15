这段文本是关于名为“Adam Blog 2.0”的Jekyll主题博客项目的介绍，包括其文件目录结构、功能特性、安装步骤以及一些额外的文档说明等内容。以下是将其中可改写成中文的部分进行改写后的内容：

# Jekyll主题：Adam Blog 2.0
由[阿曼多·梅内斯](https://github.com/amaynez)基于[阿尔乔姆·谢卢德科](https://github.com/artemsheludko)的[V1.0版本](https://github.com/artemsheludko/adam-blog)开发。

Adam Blog 2.0是一个Jekyll主题，可100%兼容[GitHub Pages](https://pages.github.com/)。如果你对GitHub Pages不熟悉，可以查看[其文档](https://help.github.com/categories/github-pages-basics/)以获取更多信息。[乔纳森·麦格隆](http://jmcglone.com/guides/github-pages/)关于在GitHub上创建和托管个人网站的指南也是一个很好的资源。

### 什么是Jekyll？

Jekyll是一个简单的、适用于博客的静态网站生成器，可用于个人、项目或组织网站。基本上，Jekyll会将你的页面内容与模板文件相结合，生成一个完整的网站。欲了解更多信息，请访问[Jekyll官方网站](https://jekyllrb.com/docs/home/)查看其文档。Codecademy也为完全的初学者提供了一门关于[如何部署Jekyll网站](https://www.codecademy.com/learn/deploy-a-website)的优秀课程。

### 从未使用过Jekyll？

在GitHub上托管你的网站的好处是，你实际上不必在计算机上安装Jekyll。一切都可以通过GitHub代码编辑器完成，只需对如何使用Jekyll或命令行有最少的了解。你所要做的就是将你的文章添加到`_posts`目录中，并编辑`_config.yml`文件以更改网站设置。只要具备一些基本的HTML和CSS知识，你甚至可以根据自己的喜好修改网站。所有这些都可以通过GitHub代码编辑器完成，它就像一个内容管理系统（CMS）一样。

## v2.0的特性：
- 搜索引擎优化（SEO）元标签
- 暗黑模式（[在_config.yml文件中可配置](https://github.com/the-mvm/the-mvm.github.io/blob/a8d4f781bfbc4107b4842433701d28f5bbf1c520/_config.yml#L10)）
- 自动生成的[sitemap.xml](http://the-mvm.github.io/sitemap.xml)
- 自动生成的[归档页面](http://the-mvm.github.io/archive/)，具备无限滚动功能
- [新页面](https://the-mvm.github.io/tag/?tag=Coding)，可按单个标签过滤文章（无需分页器V2的自动页面），也具备无限滚动功能
- 点击即可发推文功能（只需在你的Markdown中添加一个`<tweet> </tweet>`标签）
- 自定义且响应式的[404页面](https://the-mvm.github.io/404.html)
- 响应式且自动生成的目录（每篇文章可选择是否显示）
- 每篇文章的阅读时间自动计算
- 响应式的文章标签和社交分享图标（固定或内联）
- 包含领英（LinkedIn）、Reddit和Bandcamp图标
- *复制链接到剪贴板*的分享选项（及图标）
- 查看GitHub链接按钮（每篇文章可选择是否显示）
- MathJax支持（每篇文章可选择是否使用）
- 主页上的标签云
- “返回顶部” 按钮
- 评论 “窗帘”，在用户点击之前隐藏Disqus界面（[在_config.yml中可配置](https://github.com/the-mvm/the-mvm.github.io/blob/d4a67258912e411b639bf5acd470441c4c219544/_config.yml#L13)）
- [CSS变量](https://github.com/the-mvm/the-mvm.github.io/blob/d4a67258912e411b639bf5acd470441c4c219544/assets/css/main.css#L8)，便于自定义所有颜色和字体
- 添加了多个用于代码语法高亮的主题[可从_config.yml文件配置](https://github.com/the-mvm/the-mvm.github.io/blob/e146070e9348c2e8f46cb90e3f0c6eb7b59c041a/_config.yml#L44)。
- 响应式页脚菜单和页脚徽标（[如果在配置文件中设置](https://github.com/the-mvm/the-mvm.github.io/blob/d4a67258912e411b639bf5acd470441c4c219544/_config.yml#L7)）
- 搜索结果基于文章的完整内容，而不仅仅是文章描述
- 更流畅的菜单动画

## 从v1.0保留的特性
- [谷歌字体](https://fonts.google.com/)
- [Font Awesome图标](http://fontawesome.io/)
- [Disqus](https://disqus.com/)
- [MailChimp](https://mailchimp.com/)
- [谷歌分析](https://analytics.google.com/analytics/web/)
- [搜索功能](https://github.com/christian-fei/Simple-Jekyll-Search)

## 演示

[查看主题实际效果](https://the-mvm.github.io/)

主页看起来是这样的：

<img width="640px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/homepage-responsive.jpg?raw=true">

主菜单中的暗黑模式选择器：

<img width="560px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/light-toggle.png?raw=true">

文章页面看起来是这样的：

<img width="540px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/post.jpg?raw=true">
<img width="540px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/post_bottom.jpg?raw=true">

自定义的响应式404页面：

<img width="640px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/404-responsive.jpg?raw=true">

暗黑模式看起来是这样的：

<img width="640px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/homepage-dark.png?raw=true">

<img width="640px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/post-dark.png?raw=true">
<img width="640px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/post_bottom-dark.png?raw=true">

# 安装

## 本地安装

要在本地完整安装Adam Blog 2.0，[下载你自己的Adam Blog 2.0副本](https://github.com/the-mvm/the-mvm.github.io/archive/refs/heads/main.zip)并将其解压到自己的目录中。从那里，打开你喜欢的命令行工具，输入`bundle install`，然后输入`jekyll serve`。你的网站应该会在本地[http://localhost:4000](http://localhost:4000)上启动并运行。

如果你是Jekyll的完全新手，我建议查看<https://jekyllrb.com/>上的文档，或者阅读[Smashing Magazine](https://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/)的教程。

如果你在GitHub Pages上托管你的网站，那么对`_config.yml`文件（或任何其他文件）提交更改将强制使用Jekyll重建你的网站。所做的任何更改应该很快就可以看到。如果你在本地托管你的网站，那么你必须再次运行`jekyll serve`才能使更改生效。

前往`_posts`目录查看网站上当前的所有文章，并查看文章文件通常的样子的示例。你可以简单地复制模板文章并开始添加你自己的内容。

## GitHub Pages安装

### **步骤1.**
[将此存储库](https://github.com/the-mvm/the-mvm.github.io/fork/)分叉到你自己的账户。

#### 使用GitHub Pages

你可以使用GitHub Pages免费托管你的Jekyll网站。[点击此处](https://pages.github.com/)获取更多信息。

分叉时，如果你使用名为``USERNAME.github.io``的存储库作为目标，那么你的网址将是``https://USERNAME.github.io/``，否则是``https://USERNAME.github.io/REPONAME/``，并且你的网站将发布到gh-pages分支。注意：如果你在同一个GitHub用户名下列托管多个网站，那么你将不得不使用[项目页面而不是用户页面](https://help.github.com/articles/user-organization-and-project-pages/)，只需将存储库名称更改为除'http://USERNAME.github.io'之外的其他名称。

##### 如果你使用gh-pages分支，需进行的配置调整

除了映射到根网址的github-username.github.io存储库之外，你还可以通过为其他存储库使用gh-pages分支来提供网站服务，这样它们就可以在github-username.github.io/repo-name访问。

这将要求你像这样修改`_config.yml`：

```yml
# 网站设置
title: 存储库名称
email: your_email@example.com
author: 你的名字
description: "存储库描述"
baseurl: "/repo-name"
url: "https://github-username.github.io"
```

这将确保为你的资产和文章构建正确的相对路径。

### **步骤2.**
使用你的数据修改位于根目录的``_config.yml``文件。

```YAML
# 网站设置
title: 你的网站标题
description: '你的博客描述'
permalink: ':title:output_ext' # 永久链接的表现形式
baseurl: "/" # 你网站的子路径，例如 /blog
url: "" # 你网站的基本主机名和协议，例如 http://example.com
logo: "" # 你网站的徽标
logo-icon: "" # 较小的徽标，通常为正方形
logo-icon-SEO: "" # 必须是非SVG文件，可以与logo-icon相同

# 夜间/暗黑模式，默认模式是“auto”，“auto”表示自动夜间切换（19:00 - 07:00），“manual”表示手动切换，“on/off”表示默认开启/关闭。无论用户的选择是什么，它都将取代网站的默认设置，并在访问期间（会话期间）保持。只有当暗黑模式设置为“manual”时，每次访问（即无论浏览器是否关闭）它都会一直保持开启状态
night_mode: "auto"
logo-dark: "/assets/img/branding/MVM-logo-full-dark.svg" # 如果你想在暗黑模式下显示不同的徽标
highlight_theme: syntax-base16.monokai.dark # 如果需要，为代码高亮选择一个暗黑主题


# 作者设置
author: 你的名字 # 添加你的名字
author-pic: '' # 你的照片
about-author: '' # 对你的简要描述

# 联系链接
email: your@email.com # 添加你的电子邮件地址
phone: # 添加你的电话号码
website:  # 添加你的网站
linkedin:  # 添加你的领英账号
github:  # 添加你的GitHub账号
twitter:  # 添加你的推特账号
bandcamp:  # 添加你的Bandcamp用户名

# 追踪器
analytics: # 谷歌分析标签ID
fbadmin: # 脸书ID管理员

# 分页
paginate: 6 # 主页上显示的项目数量
paginate_path: 'page:num'
words_per_minute: 200 # 计算博客文章阅读时间时考虑的默认每分钟字数
```
### **步骤3.**
要配置新闻通讯，请在https://mailchimp.com上创建一个账户，设置一个网页注册表单，并将嵌入注册表单的链接粘贴到`config.yml`文件中：
```YAML
# 新闻通讯
mailchimp: "https://github.us1.list-manage.com/subscribe/post?u=8ece198b3eb260e6838461a60&amp;id=397d90b5f4"
```

### **步骤4.**
要配置Disqus，请设置一个[Disqus网站](https://disqus.com/admin/create/)，名称与你的网站相同。然后，在`_config.yml`中，编辑`disqus_identifier`值以启用。
```YAML
# Disqus
discus_identifier:  # 添加你的Disqus标识符
comments_curtain: yes # 留空以直接显示Disqus嵌入内容
```
有关[如何设置Disqus](http://www.perfectlyrandom.org/2014/06/29/adding-disqus-to-your-jekyll-powered-github-pages/)的更多信息。

### **步骤5.**
自定义网站颜色。按如下方式修改`/assets/css/main.css`：
```CSS
html {
  --shadow:       rgba(32,30,30,.3);
  --accent:       #DB504A;    /* 强调色 */
  --accent-dark:  #4e3e51;    /* 强调色2（深色） */
  --main:         #326273;    /* 主色 */
  --main-dim:     #879dab;    /* 主色的暗淡版本 */
  --text:         #201E1E;
  --grey1:        #5F5E58;
  --grey2:        #8D897C;
  --grey3:        #B4B3A7;
  --grey4:        #DAD7D2;
  --grey5:        #F0EFED;
  --background:   #ffffff;
}

html[data-theme="dark"]  {
  --accent:       #d14c47;    /* 强调色 */
  --accent-dark:  #CD8A7A;    /* 强调色2（深色） */
  --main:         #4C6567;    /* 主色 */
  --main-dim:     #273335;    /* 主色的暗淡版本 */
  --text:         #B4B3A7;
  --grey1:        #8D897C;
  --grey2:        #827F73;
  --grey3:        #76746A;
  --grey4:        #66645D;
  --grey5:        #4A4945;
  --background:   #201E1E;
  --shadow:       rgba(180,179,167,.3);
}
```
### **步骤6.**
自定义网站字体。按如下方式修改`/assets/css/main.css`：
```CSS
...
  --font1: 'Lora', charter, Georgia, Cambria, 'Times New Roman', Times, serif;/* 正文文本 */
  --font2: 'Source Sans Pro', 'Helvetica Neue', Helvetica, Arial, sans-serif; /* 标题和副标题 */
  --font1-light:      400;
  --font1-regular:    400;
  --font1-bold:       600;
  --font2-light:      200;
  --font2-regular:    400;
  --font2-bold:       700;
...
```
如果你更改了字体，你还需要按如下方式修改`/_includes/head.html`：
取消注释并使用你的新字体和字重更改以下行：
```HTML
<link href="https://fonts.googleapis.com/css?family=Lora:400,600|Source+Sans+Pro:200,400,700" rel="stylesheet">
```
删除上述行之前`<style></style>`内的所有内容：
```HTML
<style>
/* 拉丁文 */
@font-face {
  font-family: 'Lora';
  ...
</style>
```

### **步骤7.**

你会在`/_posts/`目录中找到示例文章。继续编辑任何文章并重新构建网站以查看你的更改，对于GitHub Pages，每次提交时都会自动进行此操作。你可以通过许多不同的方式重新构建网站，但最常见的方式是运行`jekyll serve`，它会启动一个Web服务器，并在文件更新时自动重新生成你的网站。

要添加新文章，只需在`_posts`目录中添加一个遵循`YYYY-MM-DD-name-of-post.md`约定的文件，并包含必要的前置内容（front matter）。查看任何示例文章以了解其工作方式。如果你已经有一个使用Jekyll构建的网站，只需复制你的文章以迁移到Adam Blog 2.0。

每篇文章的前置内容选项如下：
```YAML
---
layout: post # 确保此项保持不变
read_time: true # 根据单词数量计算并显示阅读时间
show_date: true # 显示文章的日期
title: 你的博客文章标题
date:   XXXX-XX-XX XX:XX:XX XXXX
description: "你的博客文章描述"
img: # 英雄图像的路径，来自图像文件夹（如果图像直接在图像文件夹中，只需文件名即可）
tags: [标签, 你的, 文章]
author: 你的名字
github: username