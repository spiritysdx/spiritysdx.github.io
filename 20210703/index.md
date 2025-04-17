# hugo的Lovelt主题配置

鸣谢：

[中文主题配置](https://hugoloveit.com/zh-cn/)

## 主题文档 - 基本概念


探索 Hugo - **LoveIt** 主题的全部内容和背后的核心概念.

<!--more-->

## 1 准备

由于 Hugo 提供的便利性, [Hugo](https://gohugo.io/) 本身是这个主题唯一的依赖.

直接安装满足你操作系统 (**Windows**, **Linux**, **macOS**) 的最新版本 [:(far fa-file-archive fa-fw): Hugo (> 0.62.0)](https://gohugo.io/getting-started/installing/).

{{< admonition note "为什么不支持早期版本的 Hugo?" >}}
由于 [Markdown 渲染钩子函数](https://gohugo.io/getting-started/configuration-markup#markdown-render-hooks) 在 [Hugo 圣诞节版本](https://gohugo.io/news/0.62.0-relnotes/) 中被引入, 本主题只支持高于 **0.62.0** 的 Hugo 版本.
{{< /admonition >}}

{{< admonition tip "推荐使用 Hugo extended 版本" >}}
由于这个主题的一些特性需要将 :(fab fa-sass fa-fw): SCSS 转换为 :(fab fa-css3 fa-fw): CSS, 推荐使用 Hugo **extended** 版本来获得更好的使用体验.
{{< /admonition >}}

## 2 安装

以下步骤可帮助你初始化新网站. 如果你根本不了解 Hugo, 我们强烈建议你按照此 [快速入门文档](https://gohugo.io/getting-started/quick-start/) 进一步了解它.

### 2.1 创建你的项目

Hugo 提供了一个 `new` 命令来创建一个新的网站:

```bash
hugo new site my_website
cd my_website
```

### 2.2 安装主题

**LoveIt** 主题的仓库是: [https://github.com/dillonzq/LoveIt](https://github.com/dillonzq/LoveIt).

你可以下载主题的 [最新版本 :(far fa-file-archive fa-fw): .zip 文件](https://github.com/dillonzq/LoveIt/releases) 并且解压放到 `themes` 目录.

另外, 也可以直接把这个主题克隆到 `themes` 目录:

```bash
git clone https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

或者, 初始化你的项目目录为 git 仓库, 并且把主题仓库作为你的网站目录的子模块:

```bash
git init
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

### 2.3 基础配置 {#basic-configuration}

以下是 LoveIt 主题的基本配置:

```toml
baseURL = "http://example.org/"
# [en, zh-cn, fr, ...] 设置默认的语言
defaultContentLanguage = "zh-cn"
# 网站语言, 仅在这里 CN 大写
languageCode = "zh-CN"
# 是否包括中日韩文字
hasCJKLanguage = true
# 网站标题
title = "我的全新 Hugo 网站"

# 更改使用 Hugo 构建网站时使用的默认主题
theme = "LoveIt"

[params]
  # LoveIt 主题版本
  version = "0.2.X"

[menu]
  [[menu.main]]
    identifier = "posts"
    # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
    pre = ""
    # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
    post = ""
    name = "文章"
    url = "/posts/"
    # 当你将鼠标悬停在此菜单链接上时, 将显示的标题
    title = ""
    weight = 1
  [[menu.main]]
    identifier = "tags"
    pre = ""
    post = ""
    name = "标签"
    url = "/tags/"
    title = ""
    weight = 2
  [[menu.main]]
    identifier = "categories"
    pre = ""
    post = ""
    name = "分类"
    url = "/categories/"
    title = ""
    weight = 3

# Hugo 解析文档的配置
[markup]
  # 语法高亮设置 (https://gohugo.io/content-management/syntax-highlighting)
  [markup.highlight]
    # false 是必要的设置 (https://github.com/dillonzq/LoveIt/issues/158)
    noClasses = false
```

{{< admonition >}}
在构建网站时, 你可以使用 `--theme` 选项设置主题. 但是, 我建议你修改配置文件 (**config.toml**) 将本主题设置为默认主题.
{{< /admonition >}}

### 2.4 创建你的第一篇文章

以下是创建第一篇文章的方法:

```bash
hugo new posts/first_post.md
```

通过添加一些示例内容并替换文件开头的标题, 你可以随意编辑文章.

{{< admonition >}}
默认情况下, 所有文章和页面均作为草稿创建. 如果想要渲染这些页面, 请从元数据中删除属性 `draft: true`, 设置属性 `draft: false` 或者为 `hugo` 命令添加 `-D`/`--buildDrafts` 参数.
{{< /admonition >}}

### 2.5 在本地启动网站

使用以下命令启动网站:

```bash
hugo serve
```

去查看 `http://localhost:1313`.

![基本配置下的预览](basic-configuration-preview.zh-cn.png "基本配置下的预览")

{{< admonition tip >}}
当你运行 `hugo serve` 时, 当文件内容更改时, 页面会随着更改自动刷新.
{{< /admonition >}}

{{< admonition >}}
由于本主题使用了 Hugo 中的 `.Scratch` 来实现一些特性,
非常建议你为 `hugo server` 命令添加 `--disableFastRender` 参数来实时预览你正在编辑的文章页面.

```bash
hugo serve --disableFastRender
```
{{< /admonition >}}

### 2.6 构建网站

当你准备好部署你的网站时, 运行以下命令:

```bash
hugo
```

会生成一个 `public` 目录, 其中包含你网站的所有静态内容和资源. 现在可以将其部署在任何 Web 服务器上.

{{< admonition tip >}}
网站内容可以通过 [Netlify](https://www.netlify.com/) 自动发布和托管 (了解有关[通过 Netlify 进行 HUGO 自动化部署](https://www.netlify.com/blog/2015/07/30/hosting-hugo-on-netlifyinsanely-fast-deploys/) 的更多信息).
或者, 您可以使用 [AWS Amplify](https://gohugo.io/hosting-and-deployment/hosting-on-aws-amplify/), [Github pages](https://gohugo.io/hosting-and-deployment/hosting-on-github/), [Render](https://gohugo.io/hosting-and-deployment/hosting-on-render/) 以及更多...
{{< /admonition >}}

## 3 配置

### 3.1 网站配置 {#site-configuration}

除了 [Hugo 全局配置](https://gohugo.io/overview/configuration/) 和 [菜单配置](#basic-configuration) 之外, **LoveIt** 主题还允许您在网站配置中定义以下参数 (这是一个示例 `config.toml`, 其内容为默认值).

请打开下面的代码块查看完整的示例配置 :(far fa-hand-point-down fa-fw)::

```toml
[params]
  # {{< version 0.2.0 changed >}} LoveIt 主题版本
  version = "0.2.X"
  # 网站描述
  description = "这是我的全新 Hugo 网站"
  # 网站关键词
  keywords = ["Theme", "Hugo"]
  # 网站默认主题样式 ("light", "dark", "auto")
  defaultTheme = "auto"
  # 公共 git 仓库路径，仅在 enableGitInfo 设为 true 时有效
  gitRepo = ""
  # {{< version 0.1.1 >}} 哪种哈希函数用来 SRI, 为空时表示不使用 SRI
  # ("sha256", "sha384", "sha512", "md5")
  fingerprint = ""
  # {{< version 0.2.0 >}} 日期格式
  dateFormat = "2006-01-02"
  # 网站图片, 用于 Open Graph 和 Twitter Cards
  images = ["/logo.png"]

  # {{< version 0.2.0 >}} 应用图标配置
  [params.app]
    # 当添加到 iOS 主屏幕或者 Android 启动器时的标题, 覆盖默认标题
    title = "LoveIt"
    # 是否隐藏网站图标资源链接
    noFavicon = false
    # 更现代的 SVG 网站图标, 可替代旧的 .png 和 .ico 文件
    svgFavicon = ""
    # Android 浏览器主题色
    themeColor = "#ffffff"
    # Safari 图标颜色
    iconColor = "#5bbad5"
    # Windows v8-10磁贴颜色
    tileColor = "#da532c"

  # {{< version 0.2.0 >}} 搜索配置
  [params.search]
    enable = true
    # 搜索引擎的类型 ("lunr", "algolia")
    type = "lunr"
    # 文章内容最长索引长度
    contentLength = 4000
    # 搜索框的占位提示语
    placeholder = ""
    # {{< version 0.2.1 >}} 最大结果数目
    maxResultLength = 10
    # {{< version 0.2.3 >}} 结果内容片段长度
    snippetLength = 50
    # {{< version 0.2.1 >}} 搜索结果中高亮部分的 HTML 标签
    highlightTag = "em"
    # {{< version 0.2.4 >}} 是否在搜索索引中使用基于 baseURL 的绝对路径
    absoluteURL = false
    [params.search.algolia]
      index = ""
      appID = ""
      searchKey = ""

  # 页面头部导航栏配置
  [params.header]
    # 桌面端导航栏模式 ("fixed", "normal", "auto")
    desktopMode = "fixed"
    # 移动端导航栏模式 ("fixed", "normal", "auto")
    mobileMode = "auto"
    # {{< version 0.2.0 >}} 页面头部导航栏标题配置
    [params.header.title]
      # LOGO 的 URL
      logo = ""
      # 标题名称
      name = ""
      # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
      pre = ""
      # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
      post = ""
      # {{< version 0.2.5 >}} 是否为标题显示打字机动画
      typeit = false

  # 页面底部信息配置
  [params.footer]
    enable = true
    # {{< version 0.2.0 >}} 自定义内容 (支持 HTML 格式)
    custom = ''
    # {{< version 0.2.0 >}} 是否显示 Hugo 和主题信息
    hugo = true
    # {{< version 0.2.0 >}} 是否显示版权信息
    copyright = true
    # {{< version 0.2.0 >}} 是否显示作者
    author = true
    # 网站创立年份
    since = 2019
    # ICP 备案信息，仅在中国使用 (支持 HTML 格式)
    icp = ""
    # 许可协议信息 (支持 HTML 格式)
    license = '<a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>'

  # {{< version 0.2.0 >}} Section (所有文章) 页面配置
  [params.section]
    # section 页面每页显示文章数量
    paginate = 20
    # 日期格式 (月和日)
    dateFormat = "01-02"
    # RSS 文章数目
    rss = 10

  # {{< version 0.2.0 >}} List (目录或标签) 页面配置
  [params.list]
    # list 页面每页显示文章数量
    paginate = 20
    # 日期格式 (月和日)
    dateFormat = "01-02"
    # RSS 文章数目
    rss = 10

  # 主页配置
  [params.home]
    # {{< version 0.2.0 >}} RSS 文章数目
    rss = 10
    # 主页个人信息
    [params.home.profile]
      enable = true
      # Gravatar 邮箱，用于优先在主页显示的头像
      gravatarEmail = ""
      # 主页显示头像的 URL
      avatarURL = "/images/avatar.png"
      # {{< version 0.2.7 changed >}} 主页显示的网站标题 (支持 HTML 格式)
      title = ""
      # 主页显示的网站副标题
      subtitle = "这是我的全新 Hugo 网站"
      # 是否为副标题显示打字机动画
      typeit = true
      # 是否显示社交账号
      social = true
      # {{< version 0.2.0 >}} 免责声明 (支持 HTML 格式)
      disclaimer = ""
    # 主页文章列表
    [params.home.posts]
      enable = true
      # 主页每页显示文章数量
      paginate = 6
      # {{< version 0.2.0 deleted >}} 被 params.page 中的 hiddenFromHomePage 替代
      # 当你没有在文章前置参数中设置 "hiddenFromHomePage" 时的默认行为
      defaultHiddenFromHomePage = false

  # 作者的社交信息设置
  [params.social]
    GitHub = "xxxx"
    Linkedin = ""
    Twitter = "xxxx"
    Instagram = "xxxx"
    Facebook = "xxxx"
    Telegram = "xxxx"
    Medium = ""
    Gitlab = ""
    Youtubelegacy = ""
    Youtubecustom = ""
    Youtubechannel = ""
    Tumblr = ""
    Quora = ""
    Keybase = ""
    Pinterest = ""
    Reddit = ""
    Codepen = ""
    FreeCodeCamp = ""
    Bitbucket = ""
    Stackoverflow = ""
    Weibo = ""
    Odnoklassniki = ""
    VK = ""
    Flickr = ""
    Xing = ""
    Snapchat = ""
    Soundcloud = ""
    Spotify = ""
    Bandcamp = ""
    Paypal = ""
    Fivehundredpx = ""
    Mix = ""
    Goodreads = ""
    Lastfm = ""
    Foursquare = ""
    Hackernews = ""
    Kickstarter = ""
    Patreon = ""
    Steam = ""
    Twitch = ""
    Strava = ""
    Skype = ""
    Whatsapp = ""
    Zhihu = ""
    Douban = ""
    Angellist = ""
    Slidershare = ""
    Jsfiddle = ""
    Deviantart = ""
    Behance = ""
    Dribbble = ""
    Wordpress = ""
    Vine = ""
    Googlescholar = ""
    Researchgate = ""
    Mastodon = ""
    Thingiverse = ""
    Devto = ""
    Gitea = ""
    XMPP = ""
    Matrix = ""
    Bilibili = ""
    Email = "xxxx@xxxx.com"
    RSS = true # {{< version 0.2.0 >}}

  # {{< version 0.2.0 changed >}} 文章页面配置
  [params.page]
    # {{< version 0.2.0 >}} 是否在主页隐藏一篇文章
    hiddenFromHomePage = false
    # {{< version 0.2.0 >}} 是否在搜索结果中隐藏一篇文章
    hiddenFromSearch = false
    # {{< version 0.2.0 >}} 是否使用 twemoji
    twemoji = false
    # 是否使用 lightgallery
    lightgallery = false
    # {{< version 0.2.0 >}} 是否使用 ruby 扩展语法
    ruby = true
    # {{< version 0.2.0 >}} 是否使用 fraction 扩展语法
    fraction = true
    # {{< version 0.2.0 >}} 是否使用 fontawesome 扩展语法
    fontawesome = true
    # 是否在文章页面显示原始 Markdown 文档链接
    linkToMarkdown = true
    # {{< version 0.2.4 >}} 是否在 RSS 中显示全文内容
    rssFullText = false
    # {{< version 0.2.0 >}} 目录配置
    [params.page.toc]
      # 是否使用目录
      enable = true
      # {{< version 0.2.9 >}} 是否保持使用文章前面的静态目录
      keepStatic = true
      # 是否使侧边目录自动折叠展开
      auto = true
    # {{< version 0.2.0 >}} 代码配置
    [params.page.code]
      # 是否显示代码块的复制按钮
      copy = true
      # 默认展开显示的代码行数
      maxShownLines = 10
    # {{< version 0.2.0 changed >}} {{< link "https://katex.org/" KaTeX >}} 数学公式
    [params.page.math]
      enable = true
      # 默认块定界符是 $$ ... $$ 和 \\[ ... \\]
      blockLeftDelimiter = ""
      blockRightDelimiter = ""
      # 默认行内定界符是 $ ... $ 和 \\( ... \\)
      inlineLeftDelimiter = ""
      inlineRightDelimiter = ""
      # KaTeX 插件 copy_tex
      copyTex = true
      # KaTeX 插件 mhchem
      mhchem = true
    # {{< version 0.2.0 >}} {{< link "https://docs.mapbox.com/mapbox-gl-js" "Mapbox GL JS" >}} 配置
    [params.page.mapbox]
      # Mapbox GL JS 的 access token
      accessToken = ""
      # 浅色主题的地图样式
      lightStyle = "mapbox://styles/mapbox/light-v9"
      # 深色主题的地图样式
      darkStyle = "mapbox://styles/mapbox/dark-v9"
      # 是否添加 {{< link "https://docs.mapbox.com/mapbox-gl-js/api#navigationcontrol" NavigationControl >}}
      navigation = true
      # 是否添加 {{< link "https://docs.mapbox.com/mapbox-gl-js/api#geolocatecontrol" GeolocateControl >}}
      geolocate = true
      # 是否添加 {{< link "https://docs.mapbox.com/mapbox-gl-js/api#scalecontrol" ScaleControl >}}
      scale = true
      # 是否添加 {{< link "https://docs.mapbox.com/mapbox-gl-js/api#fullscreencontrol" FullscreenControl >}}
      fullscreen = true
    # {{< version 0.2.0 changed >}} 文章页面的分享信息设置
    [params.page.share]
      enable = true
      Twitter = true
      Facebook = true
      Linkedin = false
      Whatsapp = true
      Pinterest = false
      Tumblr = false
      HackerNews = false
      Reddit = false
      VK = false
      Buffer = false
      Xing = false
      Line = true
      Instapaper = false
      Pocket = false
      Digg = false
      Stumbleupon = false
      Flipboard = false
      Weibo = true
      Renren = false
      Myspace = true
      Blogger = true
      Baidu = false
      Odnoklassniki = false
      Evernote = true
      Skype = false
      Trello = false
      Mix = false
    # {{< version 0.2.0 changed >}} 评论系统设置
    [params.page.comment]
      enable = true
      # {{< link "https://disqus.com/" Disqus >}} 评论系统设置
      [params.page.comment.disqus]
        # {{< version 0.1.1 >}}
        enable = false
        # Disqus 的 shortname，用来在文章中启用 Disqus 评论系统
        shortname = ""
      # {{< link "https://github.com/gitalk/gitalk" Gitalk >}} 评论系统设置
      [params.page.comment.gitalk]
        # {{< version 0.1.1 >}}
        enable = false
        owner = ""
        repo = ""
        clientId = ""
        clientSecret = ""
      # {{< link "https://github.com/xCss/Valine" Valine >}} 评论系统设置
      [params.page.comment.valine]
        enable = false
        appId = ""
        appKey = ""
        placeholder = ""
        avatar = "mp"
        meta= ""
        pageSize = 10
        lang = ""
        visitor = true
        recordIP = true
        highlight = true
        enableQQ = false
        serverURLs = ""
        # {{< version 0.2.6 >}} emoji 数据文件名称, 默认是 "google.yml"
        # ("apple.yml", "google.yml", "facebook.yml", "twitter.yml")
        # 位于 "themes/LoveIt/assets/data/emoji/" 目录
        # 可以在你的项目下相同路径存放你自己的数据文件:
        # "assets/data/emoji/"
        emoji = ""
      # {{< link "https://developers.facebook.com/docs/plugins/comments" "Facebook 评论系统" >}}设置
      [params.page.comment.facebook]
        enable = false
        width = "100%"
        numPosts = 10
        appId = ""
        languageCode = "zh_CN"
      # {{< version 0.2.0 >}} {{< link "https://comments.app/" "Telegram Comments" >}} 评论系统设置
      [params.page.comment.telegram]
        enable = false
        siteID = ""
        limit = 5
        height = ""
        color = ""
        colorful = true
        dislikes = false
        outlined = false
      # {{< version 0.2.0 >}} {{< link "https://commento.io/" "Commento" >}} 评论系统设置
      [params.page.comment.commento]
        enable = false
      # {{< version 0.2.5 >}} {{< link "https://utteranc.es/" "Utterances" >}} 评论系统设置
      [params.page.comment.utterances]
        enable = false
        # owner/repo
        repo = ""
        issueTerm = "pathname"
        label = ""
        lightTheme = "github-light"
        darkTheme = "github-dark"
    # {{< version 0.2.7 >}} 第三方库配置
    [params.page.library]
      [params.page.library.css]
        # someCSS = "some.css"
        # 位于 "assets/"
        # 或者
        # someCSS = "https://cdn.example.com/some.css"
      [params.page.library.js]
        # someJavascript = "some.js"
        # 位于 "assets/"
        # 或者
        # someJavascript = "https://cdn.example.com/some.js"
    # {{< version 0.2.10 changed >}} 页面 SEO 配置
    [params.page.seo]
      # 图片 URL
      images = []
      # 出版者信息
      [params.page.seo.publisher]
        name = ""
        logoUrl = ""

  # {{< version 0.2.5 >}} TypeIt 配置
  [params.typeit]
    # 每一步的打字速度 (单位是毫秒)
    speed = 100
    # 光标的闪烁速度 (单位是毫秒)
    cursorSpeed = 1000
    # 光标的字符 (支持 HTML 格式)
    cursorChar = "|"
    # 打字结束之后光标的持续时间 (单位是毫秒, "-1" 代表无限大)
    duration = -1

  # 网站验证代码，用于 Google/Bing/Yandex/Pinterest/Baidu
  [params.verification]
    google = ""
    bing = ""
    yandex = ""
    pinterest = ""
    baidu = ""

  # {{< version 0.2.10 >}} 网站 SEO 配置
  [params.seo]
    # 图片 URL
    image = ""
    # 缩略图 URL
    thumbnailUrl = ""

  # {{< version 0.2.0 >}} 网站分析配置
  [params.analytics]
    enable = false
    # Google Analytics
    [params.analytics.google]
      id = ""
      # 是否匿名化用户 IP
      anonymizeIP = true
    # Fathom Analytics
    [params.analytics.fathom]
      id = ""
      # 自行托管追踪器时的主机路径
      server = ""

  # {{< version 0.2.7 >}} Cookie 许可配置
  [params.cookieconsent]
    enable = true
    # 用于 Cookie 许可横幅的文本字符串
    [params.cookieconsent.content]
      message = ""
      dismiss = ""
      link = ""

  # {{< version 0.2.7 changed >}} 第三方库文件的 CDN 设置
  [params.cdn]
    # CDN 数据文件名称, 默认不启用
    # ("jsdelivr.yml")
    # 位于 "themes/LoveIt/assets/data/cdn/" 目录
    # 可以在你的项目下相同路径存放你自己的数据文件:
    # "assets/data/cdn/"
    data = ""

  # {{< version 0.2.8 >}} 兼容性设置
  [params.compatibility]
    # 是否使用 Polyfill.io 来兼容旧式浏览器
    polyfill = false
    # 是否使用 object-fit-images 来兼容旧式浏览器
    objectFit = false

# Hugo 解析文档的配置
[markup]
  # {{< link "https://gohugo.io/content-management/syntax-highlighting" "语法高亮设置" >}}
  [markup.highlight]
    codeFences = true
    guessSyntax = true
    lineNos = true
    lineNumbersInTable = true
    # false 是必要的设置
    # ({{< link "https://github.com/dillonzq/LoveIt/issues/158" >}})
    noClasses = false
  # Goldmark 是 Hugo 0.60 以来的默认 Markdown 解析库
  [markup.goldmark]
    [markup.goldmark.extensions]
      definitionList = true
      footnote = true
      linkify = true
      strikethrough = true
      table = true
      taskList = true
      typographer = true
    [markup.goldmark.renderer]
      # 是否在文档中直接使用 HTML 标签
      unsafe = true
  # 目录设置
  [markup.tableOfContents]
    startLevel = 2
    endLevel = 6

# 作者配置
[author]
  name = "xxxx"
  email = ""
  link = ""

# 网站地图配置
[sitemap]
  changefreq = "weekly"
  filename = "sitemap.xml"
  priority = 0.5

# {{< link "https://gohugo.io/content-management/urls#permalinks" "Permalinks 配置" >}}
[Permalinks]
  # posts = ":year/:month/:filename"
  posts = ":filename"

# {{< link "https://gohugo.io/about/hugo-and-gdpr/" "隐私信息配置" >}}
[privacy]
  # {{< version 0.2.0 deleted >}} Google Analytics 相关隐私 (被 params.analytics.google 替代)
  [privacy.googleAnalytics]
    # ...
  [privacy.twitter]
    enableDNT = true
  [privacy.youtube]
    privacyEnhanced = true

# 用于输出 Markdown 格式文档的设置
[mediaTypes]
  [mediaTypes."text/plain"]
    suffixes = ["md"]

# 用于输出 Markdown 格式文档的设置
[outputFormats.MarkDown]
  mediaType = "text/plain"
  isPlainText = true
  isHTML = false

# 用于 Hugo 输出文档的设置
[outputs]
  # {{< version 0.2.0 changed >}}
  home = ["HTML", "RSS", "JSON"]
  page = ["HTML", "MarkDown"]
  section = ["HTML", "RSS"]
  taxonomy = ["HTML", "RSS"]
  <!-- taxonomyTerm = ["HTML"] -->
```

{{< admonition >}}
请注意, 本文档其他部分将详细解释其中一些参数.
{{< /admonition >}}

{{< admonition note "Hugo 的运行环境" >}}
`hugo serve` 的默认运行环境是 `development`,
而 `hugo` 的默认运行环境是 `production`.

由于本地 `development` 环境的限制,
**评论系统**, **CDN** 和 **fingerprint** 不会在 `development` 环境下启用.

你可以使用 `hugo serve -e production` 命令来开启这些特性.
{{< /admonition >}}

{{< admonition tip "关于 CDN 配置的技巧" >}}
{{< version 0.2.7 changed >}}

```toml
[params.cdn]
  # CDN 数据文件名称, 默认不启用
  # ("jsdelivr.yml")
  data = ""
````

默认的 CDN 数据文件位于 `themes/LoveIt/assets/data/cdn/` 目录.
可以在你的项目下相同路径存放你自己的数据文件: `assets/data/cdn/`.
{{< /admonition >}}

{{< admonition tip "关于社交链接配置的技巧" >}}
{{< version 0.2.0 >}}

你可以直接配置你的社交 ID 来生成一个默认社交链接和图标:

```toml
[params.social]
  Mastodon = "@xxxx"
```

生成的社交链接是 `https://mastodon.technology/@xxxx`.

或者你可以通过一个字典来设置更多的选项:

```toml
[params.social]
  [params.social.Mastodon]
    # 排列图标时的权重 (权重越大, 图标的位置越靠后)
    weight = 0
    # 你的社交 ID
    id = "@xxxx"
    # 你的社交链接的前缀
    prefix = "https://mastodon.social/"
    # 当鼠标停留在图标上时的提示内容
    title = "Mastodon"
```

所有支持的社交链接的默认数据位于 `themes/LoveIt/assets/data/social.yaml`.
你可以参考它来配置你的社交链接.
{{< /admonition >}}

![完整配置下的预览](complete-configuration-preview.zh-cn.png "完整配置下的预览")

### 3.2 网站图标, 浏览器配置, 网站清单

强烈建议你把:

* apple-touch-icon.png (180x180)
* favicon-32x32.png (32x32)
* favicon-16x16.png (16x16)
* mstile-150x150.png (150x150)
* android-chrome-192x192.png (192x192)
* android-chrome-512x512.png (512x512)

放在 `/static` 目录. 利用 [https://realfavicongenerator.net/](https://realfavicongenerator.net/) 可以很容易地生成这些文件.

可以自定义 `browserconfig.xml` 和 `site.webmanifest` 文件来设置 theme-color 和 background-color.

### 3.3 自定义样式

{{< version 0.2.8 changed >}}

{{< admonition >}}
Hugo **extended** 版本对于自定义样式是必需的.
{{< /admonition >}}

通过定义自定义 `.scss` 样式文件, **LoveIt** 主题支持可配置的样式.

包含自定义 `.scss` 样式文件的目录相对于 **你的项目根目录** 的路径为 `assets/css`.

在 `assets/css/_override.scss` 中, 你可以覆盖 `themes/LoveIt/assets/css/_variables.scss` 中的变量以自定义样式.

这是一个例子:

```scss
@import url('https://fonts.googleapis.com/css?family=Fira+Mono:400,700&display=swap&subset=latin-ext');
$code-font-family: Fira Mono, Source Code Pro, Menlo, Consolas, Monaco, monospace;
```

在 `assets/css/_custom.scss` 中, 你可以添加一些 CSS 样式代码以自定义样式.

## 4 多语言和 i18n

**LoveIt** 主题完全兼容 Hugo 的多语言模式, 并且支持在网页上切换语言.

![语言切换](language-switch.gif "语言切换")

### 4.1 兼容性 {#language-compatibility}

{{< version 0.2.10 changed >}}

| 语言 | Hugo 代码 | HTML `lang` 属性 | 主题文档 | Lunr.js 支持 |
|:---- |:----:|:----:|:----:|:----:|
| 英语 | `en` | `en` | :(far fa-check-square fa-fw): | :(far fa-check-square fa-fw): |
| 简体中文 | `zh-cn` | `zh-CN` | :(far fa-check-square fa-fw): | :(far fa-check-square fa-fw): |
| 法语 | `fr` | `fr` | :(far fa-square fa-fw): | :(far fa-check-square fa-fw): |
| 波兰语 | `pl` | `pl` | :(far fa-square fa-fw): | :(far fa-square fa-fw): |
| 巴西葡萄牙语 | `pt-br` | `pt-BR` | :(far fa-square fa-fw): | :(far fa-check-square fa-fw): |
| 意大利语 | `it` | `it` | :(far fa-square fa-fw): | :(far fa-check-square fa-fw): |
| 西班牙语 | `es` | `es` | :(far fa-square fa-fw): | :(far fa-check-square fa-fw): |
| 德语 | `de` | `de` | :(far fa-square fa-fw): | :(far fa-check-square fa-fw): |
| 塞尔维亚语 | `pl` | `pl` | :(far fa-square fa-fw): | :(far fa-square fa-fw): |
| 俄语 | `ru` | `ru` | :(far fa-square fa-fw): | :(far fa-check-square fa-fw): |
| 罗马尼亚语 | `ro` | `ro` | :(far fa-square fa-fw): | :(far fa-check-square fa-fw): |
| 越南语 | `vi` | `vi` | :(far fa-square fa-fw): | :(far fa-check-square fa-fw): |

### 4.2 基本配置

学习了 [Hugo如何处理多语言网站](https://gohugo.io/content-management/multilingual) 之后, 请在 [站点配置](#site-configuration) 中定义你的网站语言.

例如, 一个支持英语, 中文和法语的网站配置:

```toml
# [en, zh-cn, fr, pl, ...] 设置默认的语言
defaultContentLanguage = "zh-cn"

[languages]
  [languages.en]
    weight = 1
    title = "My New Hugo Site"
    languageCode = "en"
    languageName = "English"
    [[languages.en.menu.main]]
      identifier = "posts"
      pre = ""
      post = ""
      name = "Posts"
      url = "/posts/"
      title = ""
      weight = 1
    [[languages.en.menu.main]]
      identifier = "tags"
      pre = ""
      post = ""
      name = "Tags"
      url = "/tags/"
      title = ""
      weight = 2
    [[languages.en.menu.main]]
      identifier = "categories"
      pre = ""
      post = ""
      name = "Categories"
      url = "/categories/"
      title = ""
      weight = 3

  [languages.zh-cn]
    weight = 2
    title = "我的全新 Hugo 网站"
    # 网站语言, 仅在这里 CN 大写
    languageCode = "zh-CN"
    languageName = "简体中文"
    # 是否包括中日韩文字
    hasCJKLanguage = true
    [[languages.zh-cn.menu.main]]
      identifier = "posts"
      pre = ""
      post = ""
      name = "文章"
      url = "/posts/"
      title = ""
      weight = 1
    [[languages.zh-cn.menu.main]]
      identifier = "tags"
      pre = ""
      post = ""
      name = "标签"
      url = "/tags/"
      title = ""
      weight = 2
    [[languages.zh-cn.menu.main]]
      identifier = "categories"
      pre = ""
      post = ""
      name = "分类"
      url = "/categories/"
      title = ""
      weight = 3

  [languages.fr]
    weight = 3
    title = "Mon nouveau site Hugo"
    languageCode = "fr"
    languageName = "Français"
    [[languages.fr.menu.main]]
      identifier = "posts"
      pre = ""
      post = ""
      name = "Postes"
      url = "/posts/"
      title = ""
      weight = 1
    [[languages.fr.menu.main]]
      identifier = "tags"
      pre = ""
      post = ""
      name = "Balises"
      url = "/tags/"
      title = ""
      weight = 2
    [[languages.fr.menu.main]]
      identifier = "categories"
      pre = ""
      post = ""
      name = "Catégories"
      url = "/categories/"
      title = ""
      weight = 3
```

然后, 对于每个新页面, 将语言代码附加到文件名中.

单个文件 `my-page.md` 需要分为三个文件:

* 英语: `my-page.en.md`
* 中文: `my-page.zh-cn.md`
* 法语: `my-page.fr.md`

{{< admonition >}}
请注意, 菜单中仅显示翻译的页面. 它不会替换为默认语言内容.
{{< /admonition >}}

{{< admonition tip >}}
也可以使用 [文章前置参数](https://gohugo.io/content-management/multilingual#translate-your-content) 来翻译网址.
{{< /admonition >}}

### 4.3 修改默认的翻译字符串

翻译字符串用于在主题中使用的常见默认值.
目前提供[一些语言](#language-compatibility)的翻译, 但你可能自定义其他语言或覆盖默认值.

要覆盖默认值, 请在你项目的 i18n 目录 `i18n/<languageCode>.toml` 中创建一个新文件，并从 `themes/LoveIt/i18n/en.toml` 中获得提示.

另外, 由于你的翻译可能会帮助到其他人, 请花点时间通过 [:(fas fa-code-branch fa-fw): 创建一个 PR](https://github.com/dillonzq/LoveIt/pulls) 来贡献主题翻译, 谢谢!

## 5 搜索

{{< version 0.2.0 >}}

基于 [Lunr.js](https://lunrjs.com/) 或 [algolia](https://www.algolia.com/), **LoveIt** 主题支持搜索功能.

### 5.1 输出配置

为了生成搜索功能所需要的 `index.json`, 请在你的 [网站配置](#site-configuration) 中添加 `JSON` 输出文件类型到 `outputs` 部分的 `home` 字段中.

```toml
[outputs]
  home = ["HTML", "RSS", "JSON"]
```

### 5.2 搜索配置

基于 Hugo 生成的 `index.json` 文件, 你可以激活搜索功能.

这是你的 [网站配置](#site-configuration) 中的搜索部分:

```toml
[params.search]
  enable = true
  # 搜索引擎的类型 ("lunr", "algolia")
  type = "lunr"
  # 文章内容最长索引长度
  contentLength = 4000
  # 搜索框的占位提示语
  placeholder = ""
  # {{< version 0.2.1 >}} 最大结果数目
  maxResultLength = 10
  # {{< version 0.2.3 >}} 结果内容片段长度
  snippetLength = 50
  # {{< version 0.2.1 >}} 搜索结果中高亮部分的 HTML 标签
  highlightTag = "em"
  # {{< version 0.2.4 >}} 是否在搜索索引中使用基于 baseURL 的绝对路径
  absoluteURL = false
  [params.search.algolia]
    index = ""
    appID = ""
    searchKey = ""
```

{{< admonition note "怎样选择搜索引擎?" >}}
以下是两种搜索引擎的对比:

* `lunr`: 简单, 无需同步 `index.json`, 没有 `contentLength` 的限制, 但占用带宽大且性能低 (特别是中文需要一个较大的分词依赖库)
* `algolia`: 高性能并且占用带宽低, 但需要同步 `index.json` 且有 `contentLength` 的限制

{{< version 0.2.3 >}} 文章内容被 `h2` 和 `h3` HTML 标签切分来提高查询效果并且基本实现全文搜索.
`contentLength` 用来限制 `h2` 和 `h3` HTML 标签开头的内容部分的最大长度.
{{< /admonition >}}

{{< admonition tip "关于 algolia 的使用技巧" >}}
你需要上传 `index.json` 到 algolia 来激活搜索功能. 你可以使用浏览器来上传 `index.json` 文件但是一个自动化的脚本可能效果更好.
[Algolia Atomic](https://github.com/chrisdmacrae/atomic-algolia) 是一个不错的选择.
为了兼容 Hugo 的多语言模式, 你需要上传不同语言的 `index.json` 文件到对应的 algolia index, 例如 `zh-cn/index.json` 或 `fr/index.json`...
{{< /admonition >}}

## 主题文档 - 内容


了解如何在 **LoveIt** 主题中快速, 直观地创建和组织内容.

<!--more-->

## 1 内容组织 {#contents-organization}

以下是一些方便你清晰管理和生成文章的目录结构建议:

* 保持博客文章存放在 `content/posts` 目录, 例如: `content/posts/我的第一篇文章.md`
* 保持简单的静态页面存放在 `content` 目录, 例如: `content/about.md`
* 本地资源组织

{{< admonition note "本地资源引用" >}}
{{< version 0.2.10 >}}

有三种方法来引用**图片**和**音乐**等本地资源:

1. 使用[页面包](https://gohugo.io/content-management/page-bundles/)中的[页面资源](https://gohugo.io/content-management/page-resources/).
   你可以使用适用于 `Resources.GetMatch` 的值或者直接使用相对于当前页面目录的文件路径来引用页面资源.
2. 将本地资源放在 **assets** 目录中, 默认路径是 `/assets`.
   引用资源的文件路径是相对于 assets 目录的.
3. 将本地资源放在 **static** 目录中, 默认路径是 `/static`.
   引用资源的文件路径是相对于 static 目录的.

引用的**优先级**符合以上的顺序.

在这个主题中的很多地方可以使用上面的本地资源引用,
例如 **链接**, **图片**, `image` shortcode, `music` shortcode 和**前置参数**中的部分参数.

页面资源或者 **assets** 目录中的[图片处理](https://gohugo.io/content-management/image-processing/)会在未来的版本中得到支持.
非常酷的功能! :(far fa-grin-squint fa-fw):
{{< /admonition >}}

## 2 前置参数 {#front-matter}

**Hugo** 允许你在文章内容前面添加 `yaml`, `toml` 或者 `json` 格式的前置参数.

{{< admonition >}}
**不是所有**的以下前置参数都必须在你的每篇文章中设置.
只有在文章的参数和你的 [网站设置](../theme-documentation-basics#site-configuration) 中的 `page` 部分不一致时才有必要这么做.
{{< /admonition >}}

这是一个前置参数例子:

```yaml
---
title: "我的第一篇文章"
subtitle: ""
date: 2020-03-04T15:58:26+08:00
lastmod: 2020-03-04T15:58:26+08:00
draft: true
author: ""
authorLink: ""
description: ""
license: ""
images: []

tags: []
categories: []
featuredImage: ""
featuredImagePreview: ""

hiddenFromHomePage: false
hiddenFromSearch: false
twemoji: false
lightgallery: true
ruby: true
fraction: true
fontawesome: true
linkToMarkdown: true
rssFullText: false

toc:
  enable: true
  auto: true
code:
  copy: true
  # ...
math:
  enable: true
  # ...
mapbox:
  accessToken: ""
  # ...
share:
  enable: true
  # ...
comment:
  enable: true
  # ...
library:
  css:
    # someCSS = "some.css"
    # 位于 "assets/"
    # 或者
    # someCSS = "https://cdn.example.com/some.css"
  js:
    # someJS = "some.js"
    # 位于 "assets/"
    # 或者
    # someJS = "https://cdn.example.com/some.js"
seo:
  images: []
  # ...
---
```

* **title**: 文章标题.
* **subtitle**: {{< version 0.2.0 >}} 文章副标题.
* **date**: 这篇文章创建的日期时间. 它通常是从文章的前置参数中的 `date` 字段获取的, 但是也可以在 [网站配置](../theme-documentation-basics#site-configuration) 中设置.
* **lastmod**: 上次修改内容的日期时间.
* **draft**: 如果设为 `true`, 除非 `hugo` 命令使用了 `--buildDrafts`/`-D` 参数, 这篇文章不会被渲染.
* **author**: 文章作者.
* **authorLink**: 文章作者的链接.
* **description**: 文章内容的描述.
* **license**: 这篇文章特殊的许可.
* **images**: 页面图片, 用于 Open Graph 和 Twitter Cards.

* **tags**: 文章的标签.
* **categories**: 文章所属的类别.
* **featuredImage**: 文章的特色图片.
* **featuredImagePreview**: 用在主页预览的文章特色图片.

* **hiddenFromHomePage**: 如果设为 `true`, 这篇文章将不会显示在主页上.
* **hiddenFromSearch**: {{< version 0.2.0 >}} 如果设为 `true`, 这篇文章将不会显示在搜索结果中.
* **twemoji**: {{< version 0.2.0 >}} 如果设为 `true`, 这篇文章会使用 twemoji.
* **lightgallery**: 如果设为 `true`, 文章中的图片将可以按照画廊形式呈现.
* **ruby**: {{< version 0.2.0 >}} 如果设为 `true`, 这篇文章会使用 [上标注释扩展语法](#ruby).
* **fraction**: {{< version 0.2.0 >}} 如果设为 `true`, 这篇文章会使用 [分数扩展语法](#fraction).
* **fontawesome**: {{< version 0.2.0 >}} 如果设为 `true`, 这篇文章会使用 [Font Awesome 扩展语法](#fontawesome).
* **linkToMarkdown**: 如果设为 `true`, 内容的页脚将显示指向原始 Markdown 文件的链接.
* **rssFullText**: {{< version 0.2.4 >}} 如果设为 `true`, 在 RSS 中将会显示全文内容.

* **toc**: {{< version 0.2.9 changed >}} 和 [网站配置](../theme-documentation-basics#site-configuration) 中的 `params.page.toc` 部分相同.
* **code**: {{< version 0.2.0 >}} 和 [网站配置](../theme-documentation-basics#site-configuration) 中的 `params.page.code` 部分相同.
* **math**: {{< version 0.2.0 changed >}} 和 [网站配置](../theme-documentation-basics#site-configuration) 中的 `params.page.math` 部分相同.
* **mapbox**: {{< version 0.2.0 >}} 和 [网站配置](../theme-documentation-basics#site-configuration) 中的 `params.page.mapbox` 部分相同.
* **share**: 和 [网站配置](../theme-documentation-basics#site-configuration) 中的 `params.page.share` 部分相同.
* **comment**: {{< version 0.2.0 changed >}} 和 [网站配置](../theme-documentation-basics#site-configuration) 中的 `params.page.comment` 部分相同.
* **library**: {{< version 0.2.7 >}} 和 [网站配置](../theme-documentation-basics#site-configuration) 中的 `params.page.library` 部分相同.
* **seo**: {{< version 0.2.10 >}} 和 [网站配置](../theme-documentation-basics#site-configuration) 中的 `params.page.seo` 部分相同.

{{< admonition tip >}}
{{< version 0.2.10 >}}

**featuredImage** 和 **featuredImagePreview** 支持[本地资源引用](#contents-organization)的完整用法.

如果带有在前置参数中设置了 `name: featured-image` 或 `name: featured-image-preview` 属性的页面资源,
没有必要在设置 `featuredImage` 或 `featuredImagePreview`:

```yaml
resources:
- name: featured-image
  src: featured-image.jpg
- name: featured-image-preview
  src: featured-image-preview.jpg
```
{{< /admonition >}}

## 3 内容摘要

**LoveIt** 主题使用内容摘要在主页中显示大致文章信息。Hugo 支持生成文章的摘要.

![文章摘要预览](summary.zh-cn.png "文章摘要预览")

### 自动摘要拆分

默认情况下, Hugo 自动将内容的前 70 个单词作为摘要.

你可以通过在 [网站配置](../theme-documentation-basics#site-configuration) 中设置 `summaryLength` 来自定义摘要长度.

如果您要使用 [CJK]^(中文/日语/韩语) 语言创建内容, 并且想使用 Hugo 的自动摘要拆分功能，请在 [网站配置](../theme-documentation-basics#site-configuration) 中将 `hasCJKLanguage` 设置为 `true`.

### 手动摘要拆分

另外, 你也可以添加 `<!--more-->` 摘要分割符来拆分文章生成摘要.

摘要分隔符之前的内容将用作该文章的摘要.

{{< admonition >}}
请小心输入`<!--more-->` ; 即全部为小写且没有空格.
{{< /admonition >}}

### 前置参数摘要

你可能希望摘要不是文章开头的文字. 在这种情况下, 你可以在文章前置参数的 `summary` 变量中设置单独的摘要.

### 使用文章描述作为摘要

你可能希望将文章前置参数中的 `description` 变量的内容作为摘要.

你仍然需要在文章开头添加 `<!--more-->` 摘要分割符. 将摘要分隔符之前的内容保留为空. 然后 **LoveIt** 主题会将你的文章描述作为摘要.

### 摘要选择的优先级顺序

由于可以通过多种方式指定摘要, 因此了解顺序很有用. 如下:

1. 如果文章中有 `<!--more-->` 摘要分隔符, 但分隔符之前没有内容, 则使用描述作为摘要.
2. 如果文章中有 `<!--more-->` 摘要分隔符, 则将按照手动摘要拆分的方法获得摘要.
3. 如果文章前置参数中有摘要变量, 那么将以该值作为摘要.
4. 按照自动摘要拆分方法.

{{< admonition >}}
不建议在摘要内容中包含富文本块元素, 这会导致渲染错误. 例如代码块, 图片, 表格等.
{{< /admonition >}}

## 4 Markdown 基本语法

这部分内容在 [Markdown 基本语法页面](../basic-markdown-syntax/) 中介绍.

## 5 Markdown 扩展语法 {#extended-markdown-syntax}

**LoveIt** 主题提供了一些扩展的语法便于你撰写文章.

### Emoji 支持

这部分内容在 [Emoji 支持页面](../emoji-support/) 中介绍.

### 数学公式

**LoveIt** 基于 [$ \KaTeX $](https://katex.org/) 提供数学公式的支持.

在你的 [网站配置](../theme-documentation-basics#site-configuration) 中的 `[params.math]` 下面设置属性 `enable = true`,
并在文章的前置参数中设置属性 `math: true`来启用数学公式的自动渲染.

{{< admonition tip >}}
有一份 [$ \KaTeX $ 中支持的 $ \TeX $ 函数](https://katex.org/docs/supported.html) 清单.
{{< /admonition >}}

#### 公式块

默认的公式块分割符是 `$$`/`$$` 和 `\\[`/`\\]`:

```markdown
$$ c = \pm\sqrt{a^2 + b^2} $$

\\[ f(x)=\int_{-\infty}^{\infty} \hat{f}(\xi) e^{2 \pi i \xi x} d \xi \\]
```

呈现的输出效果如下:

$$ c = \pm\sqrt{a^2 + b^2} $$

\\[ f(x)=\int_{-\infty}^{\infty} \hat{f}(\xi) e^{2 \pi i \xi x} d \xi \\]

#### 行内公式

默认的行内公式分割符是  `$`/`$` 和 `\\(`/`\\)`:

```markdown
$ c = \pm\sqrt{a^2 + b^2} $ 和 \\( f(x)=\int_{-\infty}^{\infty} \hat{f}(\xi) e^{2 \pi i \xi x} d \xi \\)
```

呈现的输出效果如下:

$ c = \pm\sqrt{a^2 + b^2} $ 和 \\( f(x)=\int_{-\infty}^{\infty} \hat{f}(\xi) e^{2 \pi i \xi x} d \xi \\)

{{< admonition tip >}}
你可以在 [网站配置](../theme-documentation-basics#site-configuration) 中自定义公式块和行内公式的分割符.
{{< /admonition >}}

#### Copy-tex

**[Copy-tex](https://github.com/Khan/KaTeX/tree/master/contrib/copy-tex)** 是一个 **$ \KaTeX $** 的插件.

通过这个扩展, 在选择并复制 $ \KaTeX $ 渲染的公式时, 会将其 $ \LaTeX $ 源代码复制到剪贴板.

在你的 [网站配置](../theme-documentation-basics#site-configuration) 中的 `[params.math]` 下面设置属性 `copyTex = true` 来启用 Copy-tex.

选择并复制上一节中渲染的公式, 可以发现复制的内容为 LaTeX 源代码.

#### mhchem

**[mhchem](https://github.com/Khan/KaTeX/tree/master/contrib/mhchem)** 是一个 **$ \KaTeX $** 的插件.

通过这个扩展, 你可以在文章中轻松编写漂亮的化学方程式.

在你的 [网站配置](../theme-documentation-basics#site-configuration) 中的 `[params.math]` 下面设置属性 `mhchem = true` 来启用 mhchem.

```markdown
$$ \ce{CO2 + C -> 2 CO} $$

$$ \ce{Hg^2+ ->[I-] HgI2 ->[I-] [Hg^{II}I4]^2-} $$
```

呈现的输出效果如下:

$$ \ce{CO2 + C -> 2 CO} $$

$$ \ce{Hg^2+ ->[I-] HgI2 ->[I-] [Hg^{II}I4]^2-} $$

### 字符注音或者注释 {#ruby}

**LoveIt** 主题支持一种 **字符注音或者注释** Markdown 扩展语法:

```markdown
[Hugo]{?^}(一个开源的静态网站生成工具)
```

呈现的输出效果如下:

[Hugo]^(一个开源的静态网站生成工具)

### 分数 {#fraction}

{{< version 0.2.0 >}}

**LoveIt** 主题支持一种 **分数** Markdown 扩展语法:

```markdown
[浅色]{?/}[深色]

[99]{?/}[100]
```

呈现的输出效果如下:

[浅色]/[深色]

[90]/[100]

### Font Awesome {#fontawesome}

**LoveIt** 主题使用 [Font Awesome](https://fontawesome.com/) 作为图标库.
你同样可以在文章中轻松使用这些图标.

从 [Font Awesome 网站](https://fontawesome.com/icons?d=gallery) 上获取所需的图标 `class`.

```markdown
去露营啦! {?:}(fas fa-campground fa-fw): 很快就回来.

真开心! {?:}(far fa-grin-tears):
```

呈现的输出效果如下:

去露营啦! :(fas fa-campground fa-fw): 很快就回来.

真开心! :(far fa-grin-tears):

