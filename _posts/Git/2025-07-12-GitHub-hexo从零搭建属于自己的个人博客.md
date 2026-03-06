---
title: GitHub-hexo从零搭建属于自己的个人博客
date: 2025-07-12
description: GitHub + hexo从零搭建属于自己的个人博客
tags: [Git, git]
categories: [Git]
---
# GitHub + hexo从零搭建属于自己的个人博客



#### 安装前提


安装Hexo之前，必须保证自己的电脑中已经安装好了**Node.js**和**Git**。因为这两个软件我之前都安装过，这里就不重复安装过程了，检验方式如下：



![1574647422567-451c8f23-e946-48f2-8fd3-21f8dc4a0947.png](/assets/img/posts/Git/1574647422567-451c8f23-e946-48f2-8fd3-21f8dc4a0947-229284.png)



#### 安装Hexo


安装好**node.js**和**git**后，可以通过**npm**来安装**Hexo**。



```plain
npm install -g hexo-cli
```



#### 建站


之后就可以在电脑里新建一个文件夹来作为存放博客全部内容的大本营了。我们直接用hexo命令来初始化博客文件夹：



```plain
hexo init <folder>
cd <folder>
npm install
```



就是文件夹的名字，我们可以自己随意取这个名字，我的经验是，现在初始化应该不需要后面**npm install**这个步骤了，在创建的时候 ，文件夹初始化已经把需要的内容都下载进去了。



![1574647433610-cd7e2c90-176e-4a35-adfb-740ffb23a8eb.png](/assets/img/posts/Git/1574647433610-cd7e2c90-176e-4a35-adfb-740ffb23a8eb-633409.png)



#### 站内内容


新建好的文件夹目录如下：



```plain
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```



这里解释一下各个文件夹的作用：



##### config.yml


博客的配置文件，博客的名称、关键词、作者、语言、博客主题...设置都在里面。



##### package.json


应用程序信息，新添加的插件内容也会出现在这里面，我们可以不修改这里的内容。



##### scaffolds


scaffolds就是脚手架的意思，这里放了三个模板文件，分别是新添加博客文章（posts）、新添加博客页（page）和新添加草稿（draft）的目标样式。  
这部分可以修改的内容是，我们可以在模板上添加比如categories等自定义内容



##### source


source是放置我们博客内容的地方，里面初始只有两个文件夹，一个是drafts（草稿），一个posts（文章），但之后我们通过命令新建tags（标签）还有categories（分类）页后，这里会相应地增加文件夹。



##### themes


放置主题文件包的地方。Hexo会根据这个文件来生成静态页面。  
初始状态下只有landscape一个文件夹，后续我们可以添加自己喜欢的。



#### Hexo命令


**init**



新建一个网站。



```xml
hexo init <folder>
```



**new**



新建文章或页面。



```plain
hexo new <layout> "title"
```



这里的对应我们要添加的内容，如果是posts就是添加新的文章，如果是page就是添加新的页面。



默认是添加posts。



然后我们就可以在对应的posts或drafts文件夹里找到我们新建的文件，然后在文件里用Markdown的格式来写作了。



**generate**



生成静态页面



```verilog
hexo generate
```



也可以简写成



```plain
hexo g
```



**deploy**



将内容部署到网站



```plain
hexo deploy
```



也可以简写成



```bash
hexo -d
```



**publish**



发布内容，实际上是将内容从drafts（草稿）文件夹移到posts（文章）文件夹。



```xml
hexo publish <layout> <filename>
```



**server**



启动服务器，默认情况下，访问网站为 `http://localhost:4000/`



```plain
hexo server
```



也可以简写成



```plain
hexo s
```



> 根据我的经验，除了第一次部署的时候，我们会重点用到**hexo init**这个命令外，在平时写博客和发布过程中最常用的就是：
> + hexo n  新建文章
> + hexo s 启动服务器，在本地查看内容
> + hexo g 生成静态页面
> + hexo deploy 部署到网站
> 以上四个步骤。



其实以上命令我觉得就足够了，文档里还有很多功能，但我在实际使用的过程中都还没有遇到。  
搭建好后我们在_localhost:4000_就可以看到这样的博客内容：



![1574647462938-7338c1d7-acbf-48c6-8b67-ebd76c4df12e.png](/assets/img/posts/Git/1574647462938-7338c1d7-acbf-48c6-8b67-ebd76c4df12e-061990.png)



### 实际操作


我在新建博客之后，做了以下改动：



#### 1. 创建“分类”页面


+ 新建分类页面



```plain
hexo new page categories
```



+ 给分类页面添加类型



我们在source文件夹中的categories文件夹下找到**index.md**文件，并在它的头部加上**type**属性。



```yaml
---
title: 文章分类
date: 2025-07-12 13:47:40
type: "categories"   #这部分是新添加的
---
```



+ 给模板添加分类属性



现在我们打开scarffolds文件夹里的post.md文件，给它的头部加上**categories**:，这样我们创建的所有新的文章都会自带这个属性，我们只需要往里填分类，就可以自动在网站上形成分类了。



```plain
{% raw %}
title: {{ title }}
{% endraw %}
{% raw %}
date: {{ date }}
{% endraw %}
categories:
tags:
```



+ 给文章添加分类



现在我们可以找到一篇文章，然后尝试给它添加分类



```plain
layout: posts
title: JavaScript学习笔记
date: 2025-07-12 00:38:36
categories: 学习笔记
tags: [node.js, express]
```



#### 2. 创建“标签”页面


创建"标签"页的方式和创建“分类”一样。



+ 新建“标签”页面



```plain
hexo new page tags
```



+ 给标签页面添加类型



我们在**source**文件夹中的**tags**文件夹下找到**index.md**文件，并在它的头部加上**type**属性。



```plain
title: tags
date: 2025-07-12 22:48:29
type: "tags" #新添加的内容
```



+ 给文章添加标签



有两种写法都可以，第一种是类似数组的写法，把标签放在中括号[]里，用英文逗号隔开



```plain
layout: posts
title: 写给小白的express学习笔记
date: 2025-07-12 00:38:36
categories: 学习笔记
tags: [node.js, express]
```



第二种写法是用-短划线列出来



```plain
layout: posts
title: 写给小白的express学习笔记
date: 2025-07-12 00:38:36
categories: 学习笔记
tags: 
- node.js
- express
```



### 部署域名


紧接着我们就可以把这些内容添加到**Github**页面上，然后生成我们自己的博客了。



#### 部署Github


+ 首先你必须有一个github账号
+ 然后新建一个仓库，这一有第一个坑，我之前用了hexoblog来作为项目名称，一直没能搭建成功，后来看到其他大牛的经验，才发现项目名一定要是用户名 **.github.io** 的形式(README.md可选可不选)



![1574647479445-457e7836-60eb-488c-b578-3a5fcdd545a6.png](/assets/img/posts/Git/1574647479445-457e7836-60eb-488c-b578-3a5fcdd545a6-301156.png)



+ 然后在setting里添加生成页面的选项



![1574647486142-b147ea89-1ad7-477c-a556-1760991ddc80.png](/assets/img/posts/Git/1574647486142-b147ea89-1ad7-477c-a556-1760991ddc80-060120.png)





![1574647495572-a438c404-b702-4d4e-bd9d-783f8aadd8ca.png](/assets/img/posts/Git/1574647495572-a438c404-b702-4d4e-bd9d-783f8aadd8ca-452867.png)



+ 这个时候github页面其实就生成好了，但是我们的内容还需要同步到github上，所以打开hexo文件夹里的配置文件**config.yml**，添加部署路径



![1574647531989-f060806e-6d81-4d82-b445-c1f6b6dc9257.png](/assets/img/posts/Git/1574647531989-f060806e-6d81-4d82-b445-c1f6b6dc9257-291822.png)



这里注意两小点：



1.属性和内容之间一定要有一个空格，配置文件有自己的格式规范



2.如果你之前没有用git关联过自己的github库，需要配置SSH等参数，否则无法成功，这部分搜git就有很多相关教程



+ 我们再用**hexo g && hexo deploy**就能将内容推送到github上了，在github页面上也能看到自己的内容了



![1574647570952-d5ffc41f-0b81-414d-8827-7785b318f7d7.png](/assets/img/posts/Git/1574647570952-d5ffc41f-0b81-414d-8827-7785b318f7d7-128564.png)



#### 部署自己的域名


+ 首先我们需要获取一个域名，我是在阿里云上购买了，上面可以根据自己想要的内容搜，比如我用了自己的名字，推荐给你的域名根据后缀不同会有价格上的区别，我选了一个不太贵的；
+ 购买域名之后需要实名认证，这是另一个坑，我之前不知道实名认证审核完成前域名无法用，一直以为自己搭建失败了；
+ 认证成功后需要解析域名



![1574647583754-300773ac-db9e-4785-90d4-5986b3514c66.png](/assets/img/posts/Git/1574647583754-300773ac-db9e-4785-90d4-5986b3514c66-607581.png)



![1574647591248-b8e3130d-815f-4b71-a423-70c7bb9af8e1.png](/assets/img/posts/Git/1574647591248-b8e3130d-815f-4b71-a423-70c7bb9af8e1-762182.png)



记录类型选CNAME，记录值是自己github生成页面的地址。



+ 在博客的页面添加CNAME文件，并在里面记录自己域名的地址，将这个文件放在public文件夹下
+ 这里还有一个小坑，CNAME文件经常被覆盖，导致我们重新部署博客后，链接就不可用了，这里可以下载一个叫hexo-generator-cname的插件，这样它会自动搞定CNAME的问题，只需要第一次手动将域名添加到文件里即可



```plain
npm i hexo-generator-cname --save
```



+ 最后**hexo g && hexo deploy**就可以了



### NexT主题


hexo有很多开源的主题，我选了**NexT**，开始只是觉得很简洁清爽，后来发现它的功能挺齐全的，提前解决了很多搭建过程中会遇到的问题。这里强烈推荐一下。



首先，**NexT**也有中文[文档](https://theme-next.iissnan.com/)，然后我们就可以开始了。



#### 安装


我是用的**git clone**的方法，文档中还有其他方法



```plain
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```



#### 设置主题


在hexo根目录下的配置文件**config.yml**里设置主题



```plain
theme: next
```



#### 配置主题


接下来我们就可以来按需配置主题内容了，所有内容都在**themes/next**文件夹下的**config.yml**文件里修改。



官方文档里写的是有些配置需要将一部分代码添加到配置文件中，但其实不用，我们逐行看配置文件就会发现，有很多功能都已经放在配置文件里了，只是注释掉了，我们只需要取消注释，把需要的相关信息补全即可使用



##### 菜单栏 menu


原生菜单栏有主页、关于、分类、标签等数个选项，但是在配置文件中是注释掉的状态，这里我们自行修改注释就行



```plain
menu:
  home: / || home
  # about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
  # schedule: /schedule/ || calendar
  # sitemap: /sitemap.xml || sitemap
  # commonweal: /404/ || heartbeat
```



注意点：



+ 如果事先没有通过hexo new page 来创建页面的话，即使在配置文件中取消注释，页面也没法显示
+ 我们也可以添加自己想要添加的页面，不用局限在配置文件里提供的选择里
+ ||后面是fontAwesome里的文件对应的名称
+ menu_icons记得选enable: true（默认应该是true）



我在这部分添加了两个自定义的页面，后面在第三方插件部分我会再提到。



```plain
menu:
  home: / || home
  # about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  读书: /books || book
  电影: /movies || film
  archives: /archives/ || archive
  # schedule: /schedule/ || calendar
  # sitemap: /sitemap.xml || sitemap
  # commonweal: /404/ || heartbeat
```



##### 主题风格 schemes


主题提供了4个，我们把想要选择的取消注释，其他三个保持注释掉的状态即可。



+ Muse



![1574647605679-b01458ea-29b2-45fb-a634-9fdaf52a947a.png](/assets/img/posts/Git/1574647605679-b01458ea-29b2-45fb-a634-9fdaf52a947a-518386.png)



+ Mist



![1574647612104-5ce70aae-4981-417f-af1a-55eb2a398467.png](/assets/img/posts/Git/1574647612104-5ce70aae-4981-417f-af1a-55eb2a398467-397741.png)![1574647616817-ad0e2ab9-ea4c-4f86-b6f5-1444d230a92e.png](/assets/img/posts/Git/1574647616817-ad0e2ab9-ea4c-4f86-b6f5-1444d230a92e-555106.png)![1574647623182-db6a4244-16f9-457c-9877-a970878876fe.png](/assets/img/posts/Git/1574647623182-db6a4244-16f9-457c-9877-a970878876fe-214585.png)



+ Pisces







+ Gemini







选择主题后也可以自定义，不过我还没摸清楚有哪些地方可以自定义，等弄清楚了我再来更新。  
底部建站时间和图标修改



##### 修改主题的配置文件：


```yaml
footer:
  # Specify the date when the site was setup.
  # If not defined, current year will be used.
  since: 2018

  # Icon between year and copyright info.
  icon: snowflake-o

  # If not defined, will be used `author` from Hexo main config.
  copyright:
  # -------------------------------------------------------------
  # Hexo link (Powered by Hexo).
  powered: false

  theme:
    # Theme & scheme info link (Theme - NexT.scheme).
    enable: false
    # Version info of NexT after scheme info (vX.X.X).
    # version: false
```



我在这部分做了这样几件事：



+ 把用户的图标从小人**user**改成了雪花**snowflake-o**
+ **copyright**留空，显示成页面**author**即我的名字
+ **powered: false**把hexo的授权图片取消了
+ **theme: enable:false** 把主题的内容也取消了



这样底部信息比较简单。



##### 个人社交信息 social


在**social**里我们可以自定义自己想要在个人信息部分展现的账号，同时给他们加上图标。



```less
social:
  GitHub: https://github.com/XuQuan-nikkkki || github
  E-Mail: mailto:xuquan1225@hotmail.com || envelope
  #Google: https://plus.google.com/yourname || google
  #Twitter: https://twitter.com/yourname || twitter
  #FB Page: https://www.facebook.com/yourname || facebook
```



注意点：



+ ||后面对应的名称是fontAwesome里图标的名称，如果我们选择的账号没有对应的图标（如豆瓣、知乎），我们可以在fontAwesome库里去选择自己喜欢的图标
+ 建议不要找太新的fontAwesome图标，主题关联的库版本没有那么新，很可能显示不了或者显示一个地球



##### 网站动画效果


为了网站响应速度我们可以把网站的动画关掉



```yaml
motion:
  enable: false
```



但我觉得页面比较素，所以开了动画，选择了canvas-nest这一个，主题自带四种效果，可以选自己喜欢的。



```yaml
motion:
  enable: true
  async: true
  
# Canvas-nest
canvas_nest: true

# three_waves
three_waves: false

# canvas_lines
canvas_lines: false

# canvas_sphere
canvas_sphere: false
```



##### 评论系统


**NexT**原生支持多说、Disqus、hypercomments等多种评论系统。我选择了**Valine**。  
方法也非常简单。直接去[Leancloud官网](https://leancloud.cn/)注册，注册完了会给你一个**appid**和**appkey**，将它们填在配置文件里即可。



```yaml
valine:
  enable: true
  appid: **************
  appkey: ***************
  notify: false # mail notifier , https://github.com/xCss/Valine/wiki
  verify: false # Verification code
  placeholder: Just go go # comment box placeholder
  avatar: mm # gravatar style
  guest_info: nick,mail,link # custom comment header
  pageSize: 10 # pagination size
```



##### 统计文章字数和阅读时间


```yaml
post_wordcount:
  item_text: true
  wordcount: true  # 文章字数
  min2read: true   # 阅读时间
  totalcount: true  # 总共字数
  separated_meta: true
```



##### 统计阅读次数


这里我用的是leancloud的服务，具体方法参考NexT上的[教程](https://notes.doublemine.me/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html#%E9%85%8D%E7%BD%AELeanCloud),添加完之后效果如下：



![1574647647617-65db4626-b3dd-460d-80ae-eca8d2236841.png](/assets/img/posts/Git/1574647647617-65db4626-b3dd-460d-80ae-eca8d2236841-541865.png)



