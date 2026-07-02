# Canghai's Blog

基于 Hugo + [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 的个人博客，在官方基础上做了一些定制。


## 改动内容

### 评论
Twikoo，配置在 `config.yml` 的 `params.twikoo`，脚本走 `registry.npmmirror.com`。
- `layouts/_partials/comments.html`

### 页脚：运行时间 + 访问统计
- 显示网站运行时长，起始时间取 `params.StartDate`
- 访问统计用 [vercount](https://vercount.one)（`cn.vercount.one/js`），沿用 `busuanzi_value_*` 的 span id
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