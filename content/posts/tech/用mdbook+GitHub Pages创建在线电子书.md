---
title: "用mdbook+GitHub Pages创建在线电子书"
date: 2023-05-12T13:34:25+08:00
draft: false
categories:
- 博客
- 豫游之乐
tags:
- mdbook
- Github Actions
---




[GitHub Actions for mdBook (rust-lang/mdBook) ⚡️ Setup mdBook quickly and build your site fast. Linux (Ubuntu), macOS, and Windows are supported.](https://github.com/peaceiris/actions-mdbook)

## 准备工作

新建public仓库然后打开读写权限，具体步骤如下：

Settings——>Actions——>general——>最下面read and write permissions 

配置github actions自动化部署，具体步骤如下：

Settings——>Pages——>deploy from branch & gh-pages——>save

In Settings->Pages, choose `Deploy from a branch` (`gh-pages`) instead of `GitHub Actions`.

如果想通过Github Token来push本地文件则需要在生成token时勾选repo和workflow选项。

[自动化部署官方指南# Automated Deployment: GitHub Actions](https://github.com/rust-lang/mdBook/wiki/Automated-Deployment%3A-GitHub-Actions)

## 安装mdbook

[mdbook官方文档](https://rust-lang.github.io/mdBook/)

[mdbook 文档中文翻译](https://hellowac.github.io/mdbook-doc-zh/index.html)

### 简介

mdBook 是一个命令行工具和 Rust crate，用于使用 Markdown 创建书籍。 输出类似于 Gitbook 等工具，非常适合创建产品或 API 文档、教程、课程材料或任何需要干净、易于导航和可自定义的演示文稿的内容。 mdBook 是用 Rust 编写的； 它的性能和简单性使其非常适合用作通过自动化直接发布到托管网站（例如 GitHub Pages）的工具。 

mdBook 包括对 Markdown 预处理和用于生成 HTML 以外格式的替代渲染器的内置支持。 这些工具还支持其他功能，例如验证。 搜索 Rust 的 crates.io 是发现更多扩展的好方法。除了以上特性，mdBook 还有一个 Rust API。 这允许您编写自己的预处理器或渲染器，以及将 mdBook 功能合并到其他应用程序中。 本指南的面向开发人员部分包含更多信息和一些示例。

- 轻量级： Markdown 语法
- 搜索： 集成 search 功能
- 语法高亮： syntax highlighting
- 多个主题： Theme 自定义输出的格式
- 预先处理器： Preprocessors 预处理的扩展，比如
- 后端： Backends 选择输出的渲染格式
- 自然，还具有 Rust 加持，速度杠杠的。
- 甚至，Rust 代码 的自动测试。

### Build from source using Rust

To build the `mdbook` executable from source, you will first need to install Rust and Cargo. Follow the instructions on the [Rust installation page](https://www.rust-lang.org/tools/install). mdBook currently requires at least Rust version 1.60.

Once you have installed Rust, the following command can be used to build and install mdBook:

`cargo install mdbook`

This will automatically download mdBook from [crates.io](https://crates.io/), build it, and install it in Cargo's global binary directory (`~/.cargo/bin/` by default).

To uninstall, run the command `cargo uninstall mdbook`.


## 生成样例书籍

### 1.安装

```bash
cargo install mdbook
```

### 2.初始化

```bash
mdbook init
```

在当前文件夹下，初始化执行**mdbook init**指令，生成如下目录结构

```html
book-test/
├── book
└── src
    ├── chapter_1.md
    └── SUMMARY.md
```

-   SUMMARY.md(配置页面左侧的结构目录)

### 3.启动

```bash
mdbook serve
```

执行**mdbook serve**指令后，打开浏览器，输入：[http://localhost:3000/](https://link.zhihu.com/?target=http%3A//localhost%3A3000/)即可看到渲染页面

### 4.build

```bash
mdbook build
```

##  init 命令详解

每本新书都有一些相同的最小模板。 为此，mdBook 包含了一个 `init` 命令。

`init` 命令的用法如下：

`mdbook init`

第一次使用 `init` 命令时，会为你设置几个文件：

```md
book-test/
├── book
└── src
    ├── chapter_1.md
    └── SUMMARY.md
```

-   `src` 目录是你用 Markdown 写书的地方。 它包含所有源文件、配置文件等。
-   `book` 目录是你的书被渲染的地方。 所有输出都已准备好上传到服务器以供观众查看。
-   `SUMMARY.md` 是本书的骨架。

**提示**：从 SUMMARY.md 生成章节

当一个`SUMMARY.md`文件已经存在时，`init`命令会首先解析它并根据`SUMMARY.md`中使用的路径生成不存在的文件。 这允许您思考和创建您的书的整个结构，然后让 mdBook 为您生成它。

### 指定目录

`init` 命令可以将目录作为参数用作书的根目录而不是当前工作目录。

`mdbook init path/to/book`

### --theme

当您使用 `--theme` 参数时，默认主题将被复制到源目录中名为 `theme` 的目录中，以便您可以对其进行修改。

主题被选择性覆盖，这意味着如果您不想覆盖特定文件，只需将其删除即可使用默认文件。

### --title

指定书名。 如果未提供，交互式提示将要求提供标题。

`mdbook init --title="my amazing book"`

### --ignore

创建一个 `.gitignore` 文件，配置为忽略构建]书籍时创建的 `book` 目录。 如果未提供，交互式提示将询问是否应创建。


## 上传到Github Repo


**原文：** [Git Push Local Branch to Remote – How to Publish a New Branch in Git](https://www.freecodecamp.org/news/git-push-local-branch-to-remote-how-to-publish-a-new-branch-in-git/)

Git 分支允许你添加新功能而不会修改项目的实时版本。如果你在一个团队中工作，不同的开发人员可能在不同的分支上工作。

从长远来看，你必须将这些独立的分支推送到远程服务器。例如，GitHub、GitLab 等。

在本文中，我将向你展示如何将本地 Git 分支推送到远程服务器。你是否还没有推送并不重要。你甚至可能已经推送了你的 `main` 分支并想要推送另一个分支。我将从头开始向你展示一切。

### 如何将 Main 分支推送到远程

如果你想将 `main` 分支推送到远程，你可能是第一次推送。在尝试推送到远程之前，请确保你已执行以下命令：

-   `git init` 用于初始化本地仓库
-   `git add .` 添加本地仓库的所有文件
-   `git commit -m ‘commit message’` 保存你对这些文件所做的更改

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/20230512174406.png)

要推送 `main` 仓库，你首先必须通过运行 `git remote add <url>` 将远程服务器添加到 Git。

要确认已添加远程，请运行 `git remote -v`：

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/20230512174441.png)

要最终推送 repo，请运行 `git push -u origin <branch-name>`。  
对我来说 `main` 是那个分支的名称，所以我运行 `git branch -M main`。

如果你尚未将 Git 配置为使用凭证助手，系统将要求你提供 GitHub 用户名和 PAT（个人访问令牌）：

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/20230512174509.png)

这就是你第一次推送 `main` 分支的方式。

### 如何将新分支推送到远程

如果你有另一个工作过的分支想要推送到远程，你仍将使用 `git push` 命令，但方式略有不同。

提醒一下，要创建一个新分支，请运行 `git branch branch-name`。要切换到该分支以便在那里工作，你必须运行 `git switch branch name` 或 `git checkout branch-name`。

要将分支推送到远程服务器，请运行 `git push –u origin`。就我而言，该分支的名称是 `bug-fixes`，所以，我需要运行 `git push -u origin bug-fixes`：

![ss4-2](https://www.freecodecamp.org/news/content/images/2022/09/ss4-2.png)

要确认分支已被推送，请前往 GitHub 并单击分支下拉菜单，你应该在那里看到分支：

![ss5-2](https://www.freecodecamp.org/news/content/images/2022/09/ss5-2.png)



## 自动化部署

[代码来源](https://github.com/marketplace/actions/mdbook-action)

An example workflow `.github/workflows/gh-pages.yml` with [GitHub Actions for GitHub Pages](https://github.com/peaceiris/actions-gh-pages). For the first deployment, we have to do this operation: [First Deployment with `GITHUB_TOKEN` - peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages#%EF%B8%8F-first-deployment-with-github_token)

```yml
name: github pages

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v2

      - name: Setup mdBook
        uses: peaceiris/actions-mdbook@v1
        with:
          mdbook-version: '0.4.10'
          # mdbook-version: 'latest'

      - run: mdbook build

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./book
```

## 后记

可以用mdbook-pdf生成pdf类型图书或生成书签目录。

mdbook-pdf是用 Rust 编写的 [mdBook](https://github.com/rust-lang/mdBook) 后端，基于[headless chrome](https://github.com/atroche/rust-headless-chrome)和[Chrome开发工具协议](https://chromedevtools.github.io/devtools-protocol/tot/Page/#method-printToPDF)生成PDF。











