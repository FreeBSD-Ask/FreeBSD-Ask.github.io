# FreeBSD 从入门到跑路 VitePress 镜像项目

镜像站点：<https://docs.bsdcn.org>

---

请勿直接向本项目的子模块提交任何 PR！若要提交，请转向子模块仓库 <https://github.com/FreeBSD-Ask/FreeBSD-Ask>。

**Please do not submit any PR to submodules of this project! It should be submitted to <https://github.com/FreeBSD-Ask/FreeBSD-Ask>**

## 架构模式

gitbook.io（可以编辑、修改、创建目录和文本内容，与 GitHub 双向同步，并且提供网页预览，有子域名） <===> GitHub A（使用 GitHub Action 推送 B） --> GitHub B（作为子目录，即 docs，使用 GitHub Action 编译推送，也可以推到自己的服务器） --> GitHub Page （B 的 page）。

## 本地测试

打开 <https://github.com/FreeBSD-Ask/FreeBSD-Ask>，点击“\< \>Code”，再在弹出窗口中点击“Download ZIP”，你的浏览器应该会自动开始下载 `FreeBSD-Ask-main.zip`。

以相同的方式下载 <https://github.com/FreeBSD-Ask/FreeBSD-Ask.github.io>，即 `FreeBSD-Ask.github.io-main.zip`

先把 `FreeBSD-Ask.github.io-main.zip` 提取（解压）到桌面上，即为 `FreeBSD-Ask.github.io-main`：

```sh
FreeBSD-Ask.github.io-main>
|   .gitattributes # 用于让 github 正确识别 markdown，用于在 github 正确高亮，正确显示编程语言（Languages）的统计信息
|   .gitignore # 一些规则，用于让阻止 git 上传特定类型的文件或目录，如 node_modules
|   .gitmodules # 子模块，制定上游为 FreeBSD-Ask/FreeBSD-Ask
|   LICENSE # 许可证
|   package.json # 适用于 bun 的 package.json，里面指定了包管理器要安装的软件及版本限制
|   README.md # 本说明文件
|
+---.github # Github Action 相关
|   |   dependabot.yml # 检测 package.json 中软件有无更新，有更新时 PR
|   |
|   \---workflows
|           no-book_push_deploy.yml # 使用当前 FreeBSD-Ask.github.io 中的子模块版本而非上游进行构建，且不生成 PDF 和 EPUB 电子书 
|           no-book_sync_deploy.yml # 使用 FreeBSD-Ask 上游最新的提交版本进行构建，且不生成 PDF 和 EPUB 电子书 
|           push_deploy.yml # 使用当前 FreeBSD-Ask.github.io 中的子模块版本而非上游进行构建
|           sync_deploy.yml # 使用 FreeBSD-Ask 上游最新的提交版本进行构建
|
\---docs # 子模块目录，用于放置 FreeBSD-Ask，指向 FreeBSD-Ask main 分支特定版本的提交，通常是 main 上最新的提交。①
```

再打开文件夹 `FreeBSD-Ask.github.io-main`，打开 `FreeBSD-Ask-main.zip`，点开 `FreeBSD-Ask-main`，按 `ctrl a` 全选文件夹 `FreeBSD-Ask-main` 里所有文件，将其放到文件夹 `FreeBSD-Ask.github.io-main` 中的目录 `docs` 中①。


```sh
FreeBSD-Ask.github.io-main>
|   .gitattributes
|   .gitignore
|   .gitmodules
|   LICENSE
|   package.json
|   README.md
|
+---.github
|   |   dependabot.yml
|   |
|   \---workflows
|           no-book_push_deploy.yml
|           no-book_sync_deploy.yml
|           push_deploy.yml
|           sync_deploy.yml
|
\---docs # 放置后的样子
    |   .gitattributes # 同上
    |   .gitignore # 同上
    |   CHANGELOG-ARCHIVE.md # 普通文件，记录当前季度的重要变动
    |   CHANGELOG.md # 普通文件，记录既往所有重要变动
    |   CODE_OF_CONDUCT.md # 用于合规，CoC
    |   CONTRIBUTING.md # 贡献指南
    |   LICENSE # 许可证
    |   mu-lu.md # Github Action mulu.yml 自动同步
    |   progress.svg # Github Action Update-commit-progress.yml 动同步
    |   README.md # 主页文件
    |   SECURITY.md # 用于合规，安全报告策略
    |   SUMMARY.md # 目录文件，同时用于生成 vitepress 左侧边栏
    |
    +---.gitbook # 图片目录
    |   \---assets # 图片
    |           1-install.png
    |           1.png
    |           1011.png
    |           1012.png
    |           其他图片略
    +---.github # Github Action 相关
    |   |   .autocorrectrc
    |   |   .markdownlint.json
    |   |   auto_assign.yml
    |   |   dependabot.yml
    |   |   lychee.toml
    |   |
    |   +---ISSUE_TEMPLATE # Github issue、PR 模板
    |   |       bug_report.md
    |   |       feature_request.md
    |   |
    |   +---scripts # Github Action 相关，由相关 yml 脚本调用
    |   |       check_images.py
    |   |       update_ga4_readme.py
    |   |       update_progress.sh
    |   |
    |   \---workflows # Github Action 相关
    |           Auto-Assign.yml
    |           AutoCorrect.yml
    |           check-images.yml
    |           create-pdf.yml
    |           file-name-check.yml
    |           links.yml
    |           markdown-lint2.yml
    |           md-padding.yml
    |           mulu.yml
    |           sync-headers.yml
    |           Update-commit-progress.yml
    |           update-ga4.yml
    |
    +---.vitepress # vitepress 相关
    |   |   config.mts # vitepress 主配置文件
    |   |
    |   \---theme # vitepress 主样式配置文件
    |           custom.css # vitepress 自定义 CSS 
    |           index.js # vitepress 自定义 JS，用于导入自定义 CSS 及 vue
    |           Layout.vue # vitepress-plugin-lightbox 插件需要，自定义样式
    |
    +---di-1-zhang-zou-jin-freebsd # 具体 Markdown 章节目录
    |       di-1.1-unix.md # 具体 Markdown 文本文件
    |       di-1.2-dao-lun.md
    |       di-1.3-jie-freebsd-jian-shi.md
    |       di-1.4-Fiat-Lux.md
    |       其他文件略
```
