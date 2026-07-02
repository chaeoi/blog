# Canghai's Blog

基于 Hugo + [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 的个人博客，在官方基础上做了一些定制。


## 改动内容

### 评论
Twikoo，配置在 `config.yml` 的 `params.twikoo`，脚本走 `registry.npmmirror.com`。
- `layouts/_partials/comments.html`

### 页脚：运行时间 + 访问统计 + 版权
- 显示网站运行时长，起始时间取 `params.StartDate`
- 访问统计用 [vercount](https://vercount.one)（`cn.vercount.one/js`），沿用 `busuanzi_value_*` 的 span id
- 统计下面一行是版权 `Copyright © StartYear-当前年 | 站点名`
- config 设 `hideFooter: true` 关掉官方自带的 "© · Powered by" 那行，整个页脚改由 extend_footer 输出
- `layouts/_partials/extend_footer.html`

### KaTeX + 霞鹜文楷
- `body` 字体设为霞鹜文楷（`assets/css/extended/custom.css`），字体文件走 npmmirror
- KaTeX `0.16.11`，CDN 换成 npmmirror；仅 `math: true` 时加载
- `config.yml` 开了 goldmark `passthrough`，否则 `$a_b$` 这类带下标的公式会被 Markdown 当成斜体处理导致渲染错误
- `layouts/_partials/extend_head.html`、`assets/css/extended/custom.css`、`config.yml`

### 文章元信息
中文的「创建 / 更新 / 时长 / 字数 / 作者」，分隔符用 `|`。
- `layouts/_partials/post_meta.html`

### 文章布局
- 标签移到标题下方的 meta 行，后面跟单页访问量
- 去掉了官方底部的 post-footer（连带分享按钮、上下篇导航）
- `layouts/single.html`、`assets/css/extended/custom.css`

### 菜单外链箭头
官方会给外链菜单项加个 ↗ 箭头，用 CSS 隐藏掉。
- `assets/css/extended/custom.css`

### 首页/列表摘要
用文章 `description` 代替官方的自动摘要（`.Summary`）。
- `layouts/list.html`

### 标签云
`/tags/` 按每个标签的文章数对数加权算字号（1~1.5rem），文章多的标签更大，配合悬停放大还原标签云效果；空分类（categories/series 无词条）做了守卫避免报错。
- `layouts/taxonomy.html`

### 自定义样式
恢复的美化样式集中在这里，逐条注释。包含：加宽导航/正文、正文两端对齐、各级标题下划线、引用块绿色、行内代码橙色 + 代码块阴影、TOC 样式、标签云居中 + 悬停放大、分页按钮、卡片/Logo/按钮悬停动画、评论字号、滚动条。另有几处对官方新版的还原：日夜切换图标尺寸（官方缩成了 17×17）、首页欢迎语下方空隙（官方改成了 margin:0）。
- `assets/css/extended/custom.css`