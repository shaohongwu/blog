---
title: "搭建自己的博客"
date: 2020-04-26T12:04:30+08:00
draft: false
---



## 工具和环境 

- Golang
- Hugo
- Github
- Netlify
- 域名






## 配置
**1. go安装**

golang的[官网](https://golang.org/dl/)下载go，根据自己的平台安装.

**2. Hugo 安装**

[GitHub](https://github.com/gohugoio/hugo/releases)上面下载自己平台的安装即可

Mac 平台用 brew

``` bash
brew install hugo
```

**3.用Hugo配置博客**

```bash
hugo new site blog
```
生成的目录文件大致如下
```fallback
.
├── archetypes
│   └── default.md
├── config.toml # 配置文件入口
├── content # Markdown 文件存放入口
├── data
├── layouts # 网站页面结构管理入口
├── static # 网站静态资源存放入口
└── themes # 网站主题皮肤入口
```
配置hugo的主题模板
在Hugo[官网](https://themes.gohugo.io/) 已经给出很多模板，选一款自己喜欢的，然后下载解压到 *themes* 文件夹

copy 主题的配置

```bash
cp themes/npq-hugo/config.toml .
```

我使用的是npq hugo 你看到的会和下面一样,然后自己修改配置

```
baseURL = "https://www.example.com"
languageCode = "en-us"
title = "title"
copyright = "Copyright © 2008–2019, Steve Francia and the Hugo Authors; all rights reserved."
theme = "npq-hugo"
pygmentsUseClasses=true

[params]
  author = "your name"
  description = "your description"
  keywords = "hugo blog"
  useAvatar = true
  microBlogSection = "posts"
  displayMicroBlog = false
  displayRecent = true
  recentMax = 4
  mail = "mail@example.com"
  phone = "8888888888"
  formspreeID = "yourformspreeID"

[menu]
  [[menu.main]]
    name = "home"
    pre = "<i class=\"fas fa-home fa-sm\"></i>"
    url = "/"
    weight = -9 
  [[menu.main]]
    name = "blog"
    pre = "<i class=\"fas fa-keyboard fa-ms\"></i>"
    url = "/blog/"
    weight = -8
  [[menu.main]]
    name = "tags"
    pre = "<i class=\"fas fa-tags fa-ms\"></i>"
    url = "/tags"
    weight = -7 
  [[menu.main]]
    name = "github"
    pre = "<i class=\"fab fa-github fa-ms\"></i>"
    url = "https://github.com/yourgithubusername23434"
    weight = -6 
  [[menu.main]]
    name = "RSS"
    pre = "<i class=\"fas fa-rss fa-ms\"></i>"
    url = "/index.xml"
    weight = -4 
  [[menu.main]]
    name = "contact"
    pre = "<i class=\"far fa-envelope\"></i>"
    url = "/contact"
    weight = -1 

```

最后run 看看效果

```
hugo server -D
```

![hugo](https://raw.githubusercontent.com/saadnpq/npq-hugo/master/images/screenshot.png)





------

## 使用GitHub存储博客



GitHub广大的程序猿的爱好网站之一，首先创建一个自己的仓库然后把本地的代码上传.

```bash
git init
git add .
git commit -m "first"

git remote add origin <替换自己的git仓库>
git push -u origin master

```

![github](https://user-images.githubusercontent.com/39219403/80298943-6882da80-87c3-11ea-97f7-8691a20dc80e.png)

## Netlify部署博客

先去官网注册一个账户，然后关联我们的git仓库

![Netlify](https://user-images.githubusercontent.com/39219403/80299080-38880700-87c4-11ea-9fa3-04664b5733bd.png)

![](https://user-images.githubusercontent.com/39219403/80299146-ae8c6e00-87c4-11ea-8277-01036b31e3d2.png)

Deploy site 之后，部署成功就能看到的网站为我们的生成的访问链接

![](https://user-images.githubusercontent.com/39219403/80299192-02975280-87c5-11ea-9067-3aa6a6571fa3.png)

然后我们就可以访问的自己的博客

在部署的时候有个坑

可能会再存 错误代码255 编译问题，这需要修改编译配置 **netlify.toml**

```
  
[build]
  command = "hugo --gc --minify -b $URL"
  publish = "public"

[build.environment]
  HUGO_VERSION = "0.69.2"
  HUGO_ENABLEGITINFO = "true"

[context.production.environment]
  HUGO_ENV = "production"

[context.deploy-preview]
  command = "hugo --gc --minify --buildFuture -b $DEPLOY_PRIME_URL"

[context.branch-deploy]
  command = "hugo --gc --minify -b $DEPLOY_PRIME_URL"

[[headers]]
  for = "*.webmanifest"
  [headers.values]
    Content-Type = "application/manifest+json; charset=UTF-8"

[[headers]]
  for = "index.xml"
  [headers.values]
    Content-Type = "application/rss+xml"
```



## 配置域名

用自己的域名服务商配置域名解析

以阿里云为例

我们新增一条CNAME 解析，等几分钟生效就就可以访问了

![](https://user-images.githubusercontent.com/39219403/80299311-feb80000-87c5-11ea-9ff2-1c0f283f1c0d.png)



最后开启https访问，在项目的设置开 Netlify 为我们提供的配置

![](https://user-images.githubusercontent.com/39219403/80299376-738b3a00-87c6-11ea-9a3a-23fea01adf99.png)

## 图片

这个有很多解决方案，国内用七牛云，但是我的域名没有备案用不了，索性找到一个使用GitHub的方案

在项目的issues 里面可以上传图片，然后在Markdown 使用



## Google Analytics

配置 GA流量监控

![](https://user-images.githubusercontent.com/39219403/80299735-4ee49180-87c9-11ea-8362-ebea32b148a3.png)

配置自己的博客地址，然后会给出配置信息

![](https://user-images.githubusercontent.com/39219403/80299771-9c60fe80-87c9-11ea-89fb-519c783f8658.png)



复制我们的代码到

```
/themes/npq-hugo/layouts/_default/baseof.html
```



## 写博客

写一篇文章

```
hugo new post/test.md
```

然后会再 content目录下面生成文件，刷新浏览器界面就能看到

![](https://user-images.githubusercontent.com/39219403/80299649-a1717e00-87c8-11ea-87d4-dd7eddbda7c2.png)

基本上就这些了，其他的后续补上



文章参考

https://www.bmpi.dev/dev/guide-to-setup-blog-site-with-zero-cost-1/