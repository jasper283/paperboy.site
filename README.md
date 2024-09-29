# 小报童精选专栏导航站

![xiaobot-navigation-site](https://img-1254434880.cos.ap-shanghai.myqcloud.com/picgo/xiaobot.png)

传送门：[点击直达](https://paperboy.site/)

## 关于小报童

小报童作为一个杰出的专栏平台，汇聚了众多优秀创作者，由于去中心化和不依赖算法推荐，读者可能会难以浏览或检索这些专栏。这也是为什么会开发这个导航站的原因，本站筛选了一些优质内容，帮助读者快速预览，轻松找到有价值的信息

## 基础功能

小报童导航站，主要就是收集一些专栏展示在上面，用户点击后可以跳转到小报童官方站，进行查看和订阅。这个内容形式就很像博客列表，所以我直接从Vercel找了个模板，作为整个项目的基础。

![vercel-templates](https://img-1254434880.cos.ap-shanghai.myqcloud.com/picgovercel-templates.png)

最终选了一个很[简洁的模板](https://vercel.com/templates/next.js/nextjs-contentlayer)

来看看它用到哪些库

        ...
        "@vercel/analytics": "^1.0.0",
        "contentlayer": "^0.3.2",
        "eslint-config-next": "13.3.1",
        "next-contentlayer": "^0.3.2",
        "next-themes": "^0.2.1",
        "tailwindcss": "3.4.12",
        "typescript": "5.0.4"
        ...

*   **contentlayer**: 核心库，将markdown文本转成html代码
*   **next-themes**: 支持暗黑模式，阅读体验更好

直接运行起来的样子，特别的简洁：

![vercel-blog-starter](https://img-1254434880.cos.ap-shanghai.myqcloud.com/picgoQQ_1727149688715.png)

## 数据哪里来

网站基础框架有了，接下来就是填充数据，小报童那么多专栏，一个个手动填充是个大工程，现在这么多小报童导航站，会不会已经有人整理好并且开源了？去Github逛一下。没想到还真的有

![github-xiaobot](https://img-1254434880.cos.ap-shanghai.myqcloud.com/picgo/QQ_1727149983878.png)

简单收集整理一下，过程不表。

***

现在原始数据是有了，那如果后续我想新小报童呢，一直从github上找也不是个好办法，因为它们更新频率不固定。

能不能自己写一个采集信息的工具？

我希望在发现一个优秀的专栏时，可以一键采集该专栏的信息，数据结构大概长这样：

        {
          "columnid": "pmthinking2023",
          "image_url": "https://static.xiaobot.net/paper/2022-12-31/5/82c8c49f1004d4c44fd6e1edc62a707a.png",
          "num": { "readers": 5734, "contents": 51 },
          "title": "产品沉思录 | 第七季（完结）",
          "owner": "@shaonan × fonter",
          "description": "产品沉思录™ 是一个关于产品的知识库，也是一个 Newsletter （邮件组），始于 2017 年，累计发布近 200+ 期，涵盖几十个人物/产品/公司专题研究，累计近百万字内容。<br><br>除了点击下方「介绍」外，你还可以浏览<br>- 免费精选集：https://pmthinking.com<br>- 本年度目录：https://xiaobot.net/post/8e704723-0707-4922-96e3-950fb9788a4a<br><br>人们会被自己热爱的事物改变，而没有人因为给予而贫穷。",
          "created_at": "2022-11-16",
          "lastupdated": "2024-06-10",
          "tags": "互联网、热门、官方推荐、产品"
        },

可以发现，我想要的信息其实都在这一个页面里面

![小报童专栏页面](https://img-1254434880.cos.ap-shanghai.myqcloud.com/picgo/QQ_1727154883394.png)

爬虫这种事，当然是交给AI做啦。我要做的就是打开Web调试，找到对应的html标签，描述给AI，让它帮我生成这个爬虫程序。

![AI生成爬虫程序](https://img-1254434880.cos.ap-shanghai.myqcloud.com/picgo/QQ_1727155209254.png)

经过几轮对话和修改，终于完美实现我的需求。

至此解决了数据采集的问题。

***

现在的博客列表只能展示标题和描述，看着有点单调。不懂设计如何快速优化？答案是上[TailwindCSS](https://tailwindui.com/components)找个合适的组件。

![tailwind-component](https://img-1254434880.cos.ap-shanghai.myqcloud.com/picgo/QQ_1727161482081.png)

这个看起来挺好，直接将代码拷贝过来。填充一下数据，得到现在[首页](https://paperboy.site)的样子啦

![小报童精选 - 首页](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/4d1e22609e0e45e18e6cae78d91fadec~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAgU2FsYWTlj6_kuZA=:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMzg2NjU0NjY3MDI2NjM3In0%3D&rk3s=f64ab15b&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1728190749&x-orig-sign=f1fm0vfYkQvRTZpKJaRnIJtgSVU%3D)

## 优化网站

单纯的平铺展示几百条数据，用户依然难以找到想要的专栏。因为数据源本来就有标签，所以加个标签筛选并不是难事。依然是上[TailwindCSS]()找个过滤器组件

![tailwindui-filter](https://img-1254434880.cos.ap-shanghai.myqcloud.com/picgo/QQ_1727161845920.png)

这个很合适，可惜是付费组件，永久解锁需要299刀！要是299元我考虑下就直接付费了，299刀可不是个小数目呀。直接截图扔给ChatGPT，让它给实现吧

最终效果还不错，不说颜值，功能肯定是满足了

![小报童专栏 - 筛选](https://img-1254434880.cos.ap-shanghai.myqcloud.com/picgo/QQ_1727162100642.png)

接下来再实现一个搜索功能，在分类筛选的基础上修改，代码非常简单，只需要一行

    let result = columns.filter(item => item.title.includes(keyword) || item.description.includes(keyword) || item.owner.includes(keyword));

## 最后

至此，网站已经完成了80%。接下来就是添加一些基础页面，比如[关于本站](https://paperboy.site/about)、[隐私协议](https://paperboy.site/privacy-policy)等。

一切开发完成后，就可以部署到[Vercel](https://vercel.com/)啦。

***

网站上线并不是终点而是起点，如何做好SEO提高曝光量才是关键。写本站有一个很重要的原因就是为了学习SEO，接下来我也会发表博客记录做SEO的过程。
