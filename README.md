# 胡滨的博客（hubin.me）

Hugo + PaperMod 主题，部署在 Cloudflare Pages。

## 本地预览

需要 Hugo（extended 版，已装在 `~/workspace/tools/hugo/hugo`）：

```bash
export PATH="$HOME/workspace/tools/hugo:$PATH"
cd ~/workspace/hubin-blog
hugo server -D
```

浏览器打开 http://localhost:1313

## 写文章

在 `content/posts/` 下新建 Markdown 文件，例如：

```bash
hugo new content posts/my-new-post.md
```

把 front matter 里的 `draft: true` 改成 `draft: false`，写完 `git push` 即自动上线。

## 部署到 Cloudflare Pages（一次性设置）

1. 把本仓库 push 到你自己的 GitHub（新建私有或公开仓库均可）。
2. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/) → Workers & Pages → Create → Pages → Connect to Git，选中该仓库。
3. Build 设置：
   - Framework preset：Hugo
   - Build command：`hugo --minify`
   - Build output directory：`public`
   - Environment variables：新增 `HUGO_VERSION = 0.168.0`
4. Deploy。成功后会得到一个 `xxx.pages.dev` 临时域名。
5. 绑定自定义域名：在该 Pages 项目 → Custom domains → Set up a custom domain，输入 `hubin.me`（按提示在 GoDaddy 或 Cloudflare DNS 加 CNAME 记录；建议把 hubin.me 的 DNS 直接托管到 Cloudflare，一键搞定且免费）。

## 可选：评论（Giscus）

1. 在博客仓库开启 Discussions（Settings → Features → Discussions）。
2. 访问 https://giscus.app 按向导生成配置。
3. 把生成的 `<script>` 片段按 PaperMod 文档加入 `layouts/partials/comments.html`，
   并在 `hugo.yaml` 的 params 下加 `comments: true`。

## 插入图片

### 单张图片（简单版）
把图片放进 `static/images/`，正文里写：

```markdown
![图片说明](/images/你的图片.jpg)
```

### 单张图片（推荐版，图片跟随文章）
把文章改成文件夹形式：`content/posts/文章名/index.md`，图片直接放同一个文件夹里，正文写相对路径：

```markdown
![图片说明](photo1.jpg)
```

推荐这样组织：以后删文章时图片一起删掉，不会留垃圾文件。

## 照片流文章（一排照片网格）

1. 新建文件夹 `content/posts/照片流标题/`，里面放 `index.md` 和所有照片（jpg/png 都行）；
2. `index.md` 正文里写一行 `{{< gallery >}}`，图片会自动按文件名顺序排成响应式网格，点任意一张看大图；
3. 网格样式在 `assets/css/extended/gallery.css`，想改列数/高度改那里。

> 图片建议：单张控制在 500KB 以内（手机照片先压缩一下），不然打开会慢。

## 目录结构

```
content/posts/     # 文章（Markdown）
content/about.md   # 关于页
themes/PaperMod/   # 主题（git submodule）
hugo.yaml          # 站点配置
static/            # 静态文件（头像 avatar.jpg、图片等放这里）
```
