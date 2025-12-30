---
title: "项目代码行数统计"
date: 2025-04-05T18:34:25+08:00
description: 本文介绍浏览器翻译插件FluentRead。
draft: false
math: true
categories:
- 编程
tags:
- CLOC
---


# 前言

最近写了一个大项目，想知道这个项目有多少行代码就找了一些相关命令和工具，于是便有了这篇博文。

我使用下面的Linux命令计算Python、JavaScript、CSS和Vue文件中的代码行数，但过滤掉`node_modules`目录下的文件。

```shell
find . "(" -name "*.py" -or -name "*.vue" -or -name "*.js" -or -name "*.css" ")" -print | grep -v "node_modules"  | xargs wc -l
```

# 在项目开发中统计代码行数的6种方式

## 一、使用 `find` 和 `wc` 命令统计代码行数

在项目文件目录下通过命令行进行代码行数统计是非常方便的。你可以快速统计所有代码的总行数、每个文件的行数，甚至可以根据特定条件进行过滤，比如使用 `find` 和 `wc` 命令。

`find` 和 `wc` 是 Linux 和 Unix 系统中常用的命令。在 Windows 系统下，如果安装了 Git，便可以通过 Git Bash 工具使用这些命令，实现代码行数的统计。

Git Bash 为 Windows 用户提供一个类似 Unix shell 的命令行环境，涵盖了 Git 和 Unix 常用的基本命令，极大地便利了项目管理，提高了开发效率。

在当前的前端项目中，核心业务代码通常位于 `src` 目录下。我们只需进入该目录，执行以下命令即可进行统计：

```bash
find . -type f -print | xargs wc -l
```

这个命令将找到所有文件，并显示每个文件的代码行数以及所有文件的总行数。

### 文件格式和空行过滤

除了基本的文件统计，该命令还可以通过多种条件进行过滤。例如，若要只读取 `.js` 和 `.ts` 文件，可以使用以下命令：

```bash
find . \( -name "*.js" -or -name "*.ts" \) -print | xargs wc -l
```

若希望过滤掉代码文件中的空行，可以结合 `grep -v "^$"` 命令：

```bash
find . \( -name "*.js" -or -name "*.ts" \) -print | xargs grep -v "^$" | wc -l
```

如果希望不统计 `node_modules` 目录中的代码，可以使用以下命令进行目录过滤：

```bash
find . -name "*.js" | grep -v "node_modules" | xargs wc -l
```

### 命令参数简要说明

- **find**：在给定路径下查找匹配的文件。`-name` 表示按文件名查找，`-type` 指定文件类型。
- **wc**：统计文件的总行数、字数和字符数，`-l` 选项用于统计行数。
- **grep**：逐行搜索文件内容，找出匹配的行。`-v` 选项表示不匹配的行。

在 Windows 中，用户还可以使用 PowerShell 命令行工具进行项目代码行数的统计。

## 二、PowerShell 工具统计代码行数

在项目的 `src` 目录下，打开 PowerShell 命令行窗口，输入以下命令以统计所有文件的行数总和：

```powershell
(Get-ChildItem -Recurse -File | Get-Content | Measure-Object).Count
```

这个命令会输出 `src` 目录下所有文件的行数总和。若要排除空行，可以使用 `-Line` 参数选项：

```powershell
(Get-ChildItem -Recurse -File | Get-Content | Measure-Object -Line).Lines
```

**按文件格式过滤：**

如果要过滤特定格式的文件（如 `.ts` 和 `.js`），可以使用通配符：

```powershell
(Get-ChildItem -Recurse "*.ts","*.js" | Get-Content | Measure-Object).Count
```

由于这个过滤方式无法指定路径，因此可以结合 `Where-Object` 命令来定义路径并过滤文件格式，如下所示：

```powershell
(Get-ChildItem .\src -Recurse | Where-Object { $_.Name -like "*.ts" -or $_.Name -like "*.js" } | Get-Content | Measure-Object).Count
```

如果你希望输出每个文件的行数，可以使用以下命令：

```powershell
Get-ChildItem -Recurse -File | ForEach-Object { Write-Output "$($_.FullName) $(Get-Content $_.FullName | Measure-Object).Count" }
```

### PowerShell 相关命令简要说明

- **Get-ChildItem**：获取指定目录中文件和子目录的列表。`dir` 命令在 PowerShell 中也能实现相同的功能。
- **-Recurse**：在搜索目录时递归子目录。
- **Get-Content**：读取文本文件的内容，包括文件的行数、字符数、字节数等信息。
- **Measure-Object**：对一组对象进行测量和计算，例如求和。
- **Where-Object**：根据指定条件过滤对象集合的数据。
- **ForEach-Object**：依次对输入的对象组中的每个对象执行指定操作，适用于遍历文件。
- **Write-Output**：将指定的字符串或值输出到 PowerShell 控制台。

## 三、使用 Git 命令

除了前两种命令行统计方法，还可以使用 Git 命令来统计代码行数。命令相对简单，主要包括以下两个：

- **git ls-files**：列出项目中的所有文件，便于查看文件的代码行数和总代码行数。
- **git log**：获取提交的历史记录，通过记录统计分支下修改的代码行数。

首先，我们可以使用 `git ls-files` 统计项目中所有文件的总代码行数：

```bash
git ls-files | xargs cat | wc -l
```

**注意**：在使用 Git 统计时，确保代码已提交至 Git 仓库，否则统计命令将出错。

若要统计每个文件的代码行数，可使用以下命令：

```bash
git ls-files | xargs wc -l
```

这样会显示每个文件的行数以及总代码行数。

使用 `git ls-files` 的方式与前文提到的 `find` 命令相似，同样可以对结果进行过滤。例如，统计 `.tsx` 和 `.js` 格式的文件，并过滤掉 `src/assets` 目录下的文件，可以使用以下命令：

```bash
git ls-files '*.tsx' '*.js' | grep -v 'src/assets' | xargs wc -l
```

同样，使用 `git log` 命令来统计特定分支下修改的代码行数：

```bash
git log --no-merges --pretty=tformat: --numstat | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }'
```

该命令能够统计当前分支下的代码行数，包括新增的行数、删除的行数，以及总代码行数。

### Git log 统计参数说明

通过在 Git 命令中添加一些参数，可以实现更灵活的统计需求：

- **--no-merges**：去除提交中关于合并的代码行，因为开发人员可能会合并其他分支的代码，这时需要去除这些影响。
- **--since** 和 **--until**：定义要统计的修改记录的开始和结束时间，适用于统计某个迭代的代码行数。
- **--author**：指定修改代码的作者，多个过滤条件可以用竖线（`|`）分隔。
- **--pretty=**：控制输出记录的格式。
- **--numstat**：对增加和删除的行数进行统计，输出中第一列是增加的行数，第二列是删除的行数。

例如，统计 `master` 分支，从 2023-03-01 到 2023-03-26 之间，由 `zuozhe` 提交的所有修改代码行数，可以使用以下命令：

```bash
git log master --no-merges --since=2023-03-01 --until=2023-03-26 --author="zuozhe" --pretty=tformat: --numstat | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }'
```

这样，你就可以详细统计到在特定时间段内由特定作者修改的代码行数。希望这些方法能够帮助你高效地管理和量化代码行数。

## 四、代码编辑器插件

现代代码编辑器通常提供了一些插件，用于统计代码行数。例如，博主使用的 VS Code 就非常便利。在这里，我们介绍 VS Code Counter 插件，它能够统计项目中的代码行数、注释行数和空行数，并且以不同格式显示每个文件的行数。

### 安装和使用

首先，你需要在插件市场中安装 VS Code Counter 插件。安装完成后，使用快捷键 `Ctrl + Shift + P` 打开命令面板，输入 `VSCodeCounter: Count lines in directory`，接着会弹出输入框，默认显示当前工作目录，你可以根据需要更改为其他项目路径。

确认项目路径后，按回车键，插件会自动计算并在一个 `.md` 文件中展示所选路径下项目文件的行数统计结果。统计的结果将在当前目录下生成一个名为 `.VSCodeCounter` 的文件夹，其中包含生成的各种代码统计数据，你可以随时查询。

## 五、jscpd

在前面的文章中提到过如何使用 jscpd 工具库来统计代码重复度。值得一提的是，jscpd 也具备统计项目代码行数的功能。

使用 jscpd 后，你可以在其界面看到整个项目的代码总行数（Total 行），具体的使用细节可以参考之前的博文。

## 六、自己实现一个统计工具

除了使用上述五种方法，我们还可以亲手实现一个读取项目代码行数的工具。创建一个简单的代码行数统计工具，只需遵循以下三个步骤：

1. 迭代遍历项目目录下的所有文件。
2. 读取文件内容，使用换行符 `\n` 进行行数判断。
3. 计算总的代码行数。

对于前端开发者，使用 Node.js 完成这个任务非常方便。以下是迭代文件的基础代码：

```js
// 可以通过命令执行时传入目录参数
const fs = require('fs');
const path = require('path');

const params = process.argv.splice(2);
const root = params[0];

// 遍历读取文件
function queryAllFiles(root) {
  let stat = fs.statSync(root);
  if (stat.isDirectory()) {
    let files = fs.readdirSync(root);
    for (let file of files) {
      file = path.join(root, file);
      const dirStat = fs.statSync(file);
      if (dirStat.isDirectory()) {
        queryAllFiles(file);
      } else {
        // 读取文件行数
      }
    }
  } else {
    // 读取文件行数
  }
}
```

接下来是读取文件内容和计算行数的代码：

```js
// 读取文件内容，通过 `\n` 换行符分割内容后，统计行数
let fileContent = fs.readFileSync(path, 'utf-8');
const fileLines = fileContent.split('\n');
const lines = fileLines.length;
console.log(`${path}: ${lines} lines`);
```

通过对每个文件的行数进行累加，我们就可以统计出总的代码行数。

### 统计注释和空行

此外，你还可以统计代码中的注释行。以前端的 JS 代码为例，使用正则表达式判断一行代码是否为注释：

```js
const reg = /^(\/\/|\/\*|\*|\*\/)/;
const commentLines = fileLines.filter(line => reg.test(line.trimStart())).length;
```

这里的注释判断适用于 JS 代码中的注释样式（`//`、`/*`、`*`、和`*/`），对于其他注释风格，需要根据具体情况调整正则表达式。

判断空行也很简单，仅需检查一行是否为空：

```js
const blankLines = fileLines.filter(line => line.trim() === '').length;
```

### 忽略特定目录和文件

如果需要过滤某些目录或文件，只需在迭代文件时添加相应的条件。比如，若要忽略 `node_modules` 和 `dist` 目录，可以使用以下代码：

```js
// 定义忽略的目录
const ignores = ['node_modules', 'dist'];

// 文件迭代时忽略相应目录
for (let file of files) {
  if (ignores.some(ignore => file.includes(ignore))) {
    continue;
  }
  // ...
}
```

### 文件格式过滤

你还可以根据文件格式进行过滤，例如只统计 `.js` 和 `.ts` 格式的文件：

```js
// 只统计 js 和 ts 的代码文件
const formats = ['.js', '.ts'];
if (stat.isFile()) {
  const ext = path.extname(file);
  if (formats.includes(ext)) {
    // 读取文件内容和行数
    // ...
  }
}
```

这样，简单的统计工具就完成了，运行效果如图所示：

## 总结

以上六种方法都可以方便地统计代码行数，各自具有不同的特点。比如，`find` 命令可以在不同系统中使用，而 PowerShell 则专为 Windows 用户设计；`jscpd` 不仅能统计代码行数，还能检测代码重复度。

安装了 Git 的用户可以利用 `git log` 来统计代码的修改行数、不同人员的提交情况等。许多代码编辑器都提供了各种插件，不但可以统计代码行数，还能区分文件格式、注释行数和空行，操作直观且方便。而如果你选择自己实现统计工具，则能够根据需求扩展更多功能，甚至可以将其集成到构建工具中，满足不同场景的需求。

# CLOC（Count Lines of Code）

CLOC（Count Lines of Code）是一个强大的工具，能够统计多种编程语言中的空行、注释行和物理行数。这个工具非常实用，可以帮助我们快速了解项目中的代码信息。

> **注**：下面这个命令可以实现统计代码行数的功能，但不排除空行和注释行：

```bash
find . -name "*.php" -print | xargs wc -l
```

# 参考资料

[在项目开发中统计代码行数的 6 种方式](https://blog.csdn.net/jimojianghu/article/details/129792250)










