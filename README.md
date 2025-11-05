# FreeBSD 从入门到跑路 VitePress 镜像项目

镜像站点：<https://docs.bsdcn.org>

---

请勿直接向本项目的子模块提交任何 PR！若要提交，请转向子模块仓库 <https://github.com/FreeBSD-Ask/FreeBSD-Ask>。

**Please do not submit any PR to submodules of this project! It should be submitted to <https://github.com/FreeBSD-Ask/FreeBSD-Ask>**

## 架构模式

gitbook.io（可以编辑、修改、创建目录和文本内容，与 GitHub 双向同步，并且提供网页预览，有子域名） <===> GitHub A（使用 GitHub Action 推送 B） --> GitHub B（作为子目录，即 docs，使用 GitHub Action 编译推送，也可以推到自己的服务器） --> GitHub Page （B 的 page）。

## 本地测试

### 安装 bun

- MacOS/Linux

```bash
# curl -fsSL https://bun.com/install | bash
```

- Windows（powershell）

```powershell
# powershell -c "irm bun.sh/install.ps1|iex"
```

>**技巧**
>
>以管理员权限运行的 powershell 是不允许拖拽文件到命令行窗口的！

如有变动，参见 [Install Bun](https://bun.com/docs/installation)。

### 获取项目文件及结构

打开 <https://github.com/FreeBSD-Ask/FreeBSD-Ask>，点击“\< \>Code”，再在弹出窗口中点击“Download ZIP”，你的浏览器应该会自动开始下载 `FreeBSD-Ask-main.zip`。

以相同的方式下载 <https://github.com/FreeBSD-Ask/FreeBSD-Ask.github.io>，即 `FreeBSD-Ask.github.io-main.zip`

先把 `FreeBSD-Ask.github.io-main.zip` 提取（解压）到桌面上，即为 `FreeBSD-Ask.github.io-main`：

#### `FreeBSD-Ask.github.io-main.zip` 的简要目录结构

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
#### 拉取子模块后 `FreeBSD-Ask.github.io-main.zip` 的完整目录结构

再打开文件夹 `FreeBSD-Ask.github.io-main`，打开 `FreeBSD-Ask-main.zip`，点开 `FreeBSD-Ask-main`，按 `ctrl a` 全选文件夹 `FreeBSD-Ask-main` 里所有文件，将其放到文件夹 `FreeBSD-Ask.github.io-main` 中的目录 `docs` 中①。


- 完整的目录结构

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
    +---public # 放置网站的静态文件
    |       favicon.ico # 会生成形如 docs.bsdcn.org/favicon.ico 的访问路径
    |       其他文件从略

```

### `package.json` 简要解释

>**警告**
>
>json 中不允许注释！如果你直接复制粘贴，那么下面的 `#` 及后面的文字都将是非法字符！即显示为红色背景。

```json
{
    "type": "module", # 使用 ESM，起兼容作用
    "license": "BSD-2-Clause", # 许可证
    "dependencies": { # 主要依赖
        "vitepress": "2.0.0-alpha.12", # vitepress 本体
        "vite-plugin-vitepress-auto-nav": "3.0.0", # vitepress 侧边栏插件
        "markdown-it-footnote": "4.0.0", # vitepress 脚注
        "markdown-it-task-checkbox": "1.0.6", # vitepress 复选框
        "vitepress-plugin-pagefind": "0.4.15", # vitepress 搜索
        "pagefind": "1.4.0", # vitepress 搜索的依赖
        "vitepress-plugin-lightbox": "1.0.3", # vitepress 图片点击放大功能
        "noto-sans-sc": "37.0.0", # noto 字体
        "lxgw-wenkai-screen-web": "1.521.0",  # 霞鹜文楷字体
        "vite-plugin-sri3": "1.1.0", # SRI 合规
        "vite-plugin-csp-guard": "3.0.0" # CSP 合规，目前因未知原因不生效。①
    },
    "trustedDependencies": [ # 信任依赖，即子依赖
        "vitepress-plugin-pagefind", # vitepress 搜索
        "esbuild", # sri3 或 csp 所需
		"vue-demi" # sri3 或 csp 所需。②
    ],
    "scripts": { # 运行方法
        "docs:dev": "vitepress dev docs",
        "docs:build": "vitepress build docs && pagefind --site docs/.vitepress/dist",
        "docs:preview": "vitepress preview docs" # ③
    }
}
```

>**警告**
>
>①、②、③ 每部分的末尾不允许有英文逗号，但是前面必须要个英文逗号！语法必须正确。不限制缩进格式。

### 在浏览器安装缓存清除插件

在测试中经常会收到缓存的干扰，十分有必要安装无缓存插件。

- Chrome 在 [Cache Killer](https://chromewebstore.google.com/detail/cache-killer/mobkodffjnomdafehbljjphjaipbenpm)。
- 火狐在 [Clear Cache](https://addons.mozilla.org/en-US/firefox/addon/clearcache/)。

### 测试与运行

定位到目录 `FreeBSD-Ask.github.io-main`

-  根据 `package.json` 安装所需依赖
  
```sh
C:\Users\ykla\Desktop\FreeBSD-Ask.github.io-main>bun install
bun install v1.3.1 (89fa0f34)
warn: incorrect peer dependency "vitepress@2.0.0-alpha.12"

+ lxgw-wenkai-screen-web@1.521.0
+ markdown-it-footnote@4.0.0
+ markdown-it-task-checkbox@1.0.6
+ noto-sans-sc@37.0.0
+ pagefind@1.4.0
+ react@19.2.0
+ vite-plugin-csp-guard@3.0.0
+ vite-plugin-vitepress-auto-nav@3.0.0
+ vitepress@2.0.0-alpha.12
+ vitepress-plugin-lightbox@1.0.3
+ vitepress-plugin-pagefind@0.4.15

189 packages installed [18.33s]
```

- 热加载运行

```sh
C:\Users\ykla\Desktop\FreeBSD-Ask.github.io-main>bun run docs:dev 
🎈 SUMMARY 解析中...
🎈 SUMMARY 解析完成...

  vitepress v2.0.0-alpha.12

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h to show help
```

在网页打开 `http://localhost:5173` 即可。退出时关闭命令行窗口即可。

>**技巧**
>
>如果报错 404，是正常的，点击导航栏的“目录”即可！不要尝试自己生成 `index.md`，有可能影响构建！

现在可以实时修改 `custom.css`，浏览器网页会自动刷新修改后的结果。同时在命令行窗口中，也会提示你的修改次数，形如 `x12`（变动了 12 次）。

其他文件的变动，若修改后网页报错，请重新 `bun run docs:dev`。

### `package.json` 的变动处理 

`bun install` 后，会在命令当前所在路径下生成文件夹 `node_modules`，通常在文件夹 `FreeBSD-Ask.github.io-main` 下。当 `package.json` 发生变动时，应删除文件夹 `node_modules`，然后再 `bun install`。

### 修改后完整构建测试

在测试完成后，应进行一次完整的构建测试。

```sh
# bun run docs:build
```

