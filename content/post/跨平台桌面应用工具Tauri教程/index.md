---
title: 跨平台桌面应用工具Tauri2.0入门教程
description: 本文介绍跨平台桌面应用工具Tauri。
date: 2025-05-24T20:34:25+08:00
draft: false
math: true
categories:
- 编程
- 计算机
tags:
- Tauri
---

# 跨平台桌面应用工具Tauri 2.0入门

## 前言

_Tauri_ 是一款基于 Rust 的跨平台桌面应用开发框架，能够让开发者使用熟悉的前端技术（HTML、CSS、JavaScript 或现代框架如 React、Vue 等）来构建轻量级、高性能且安全的应用。相比 Electron，Tauri 打包体积更小（约4MB 对比 Electron 50MB），性能更优，占用资源更少。

本教程将带你从零开始创建一个基础的 Tauri 2.0 项目，包含环境搭建、项目初始化、前后端交互、构建打包等步骤，轻松打造跨平台桌面应用。

## 前提条件

在开始之前，请确保你的开发环境中已经安装并配置好以下内容：

- **Rust**  
  官网：[https://rustup.rs/](https://rustup.rs/)  
  按照页面指示安装 Rust 编译工具链。

- **Node.js 和 npm**  
  官网：[https://nodejs.org/](https://nodejs.org/)  
  建议安装最新的 LTS 版本。

- **Tauri CLI**  
  在命令行终端运行：

```bash
npm install -g @tauri-apps/cli
```

## 环境搭建

1. **创建新项目**使用官方脚手架工具快速初始化：

NPM:

```shell
npm create tauri-app@latest
```

Yarn:

```shell
yarn create tauri-app
```

PNPM:

```shell
pnpm create tauri-app
```

然后选择适合自己的前端技术栈。

2. **启动开发服务器**  执行`cd tauri-app`进入项目目录后，执行`npm install`或是`pnpm install`安装依赖，然后运行：

```bash
npm run tauri dev
```

这将启动 Tauri 应用的开发模式，内置热重载，方便实时调试。


## 创建 Tauri 应用

项目主要分为前端（Web 页面）和后端（Rust）两部分。Tauri 通过命令调用机制，让前端能与后端 Rust 代码通信。

---

### 实现前后端通信

1. 编辑 `src-tauri/src/main.rs`，加入一个简单的命令函数：

```rust
#[tauri::command]
fn greet(name: String) -> String {
    format!("Hello, {}!", name)
}

fn main() {
    tauri::Builder::default()
        .invoke_handler(tauri::generate_handler![greet])
        .run(tauri::generate_context!())
        .expect("error while running tauri application");
}
```

2. 在前端调用这个命令：

编辑 `src/app.js`：

```js
import { invoke } from '@tauri-apps/api/tauri';

async function greet(name) {
  const response = await invoke('greet', { name });
  alert(response);
}

document.getElementById('greet-btn').addEventListener('click', () => {
  greet('TauriUser');
});
```

3. 更新 `index.html`，添加一个触发按钮：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Tauri App</title>
</head>
<body>
  <button id="greet-btn">Greet</button>
  <script src="app.js"></script>
</body>
</html>
```

---

## 构建与运行

- **开发模式**（带热重载）

  ```bash
  npm run tauri dev
  ```

- **生产构建**  
  打包成正式版本：

  ```bash
  npm run tauri build
  ```

---

## 打包平台应用

Tauri 支持一键针对不同操作系统进行打包：

- Windows：

  ```bash
  npm run tauri build -- --target x86_64-pc-windows-msvc
  ```

- macOS：

  ```bash
  npm run tauri build -- --target x86_64-apple-darwin
  ```

- Linux：

  ```bash
  npm run tauri build -- --target x86_64-unknown-linux-gnu
  ```

（具体命令根据项目配置有所不同，可参考官方文档。）

---

## 总结与扩展

至此，你已经完成了一个简单的 Tauri 应用搭建过程，掌握了如何创建项目、实现前后端交互、构建及打包。Tauri 让桌面应用开发变得高效且轻量，非常适合有前端基础的开发者快速入门。

后续你可以尝试：

- 深入 Rust 后端逻辑，扩展应用功能
- 使用 Vue、React 等框架改进前端界面
- 利用 Tauri 丰富的插件系统增强应用能力
- 探索移动端的支持与多端统一开发

更多资料和示例，推荐访问 Tauri 官方网站：[https://tauri.app](https://tauri.app/) 和 [GitHub 项目](https://github.com/tauri-apps/tauri)。

# [创建 Tauri 项目](https://www.cnblogs.com/guojikun/p/18412220 "发布于 2024-09-13 15:09")

[合集 - Tauri(2)](https://www.cnblogs.com/guojikun/collections/20566)

[1. Windows 上 Tauri 开发环境配置（2024-09-11）](https://www.cnblogs.com/guojikun/p/18406284)

---

## 1. 从头开始创建一个全新 Tauri 项目

在前述部分，我们已详细介绍了如何在 Windows 上搭建 Tauri 的开发环境，包括依赖安装、工具配置，以及常见问题的处理。这为我们的后续开发打下了坚实基础。

如今，我们将学习如何从零开始创建一个完整的 Tauri 项目。这个流程非常简单，Tauri 提供了强大的 CLI 工具，让你只需几条命令就能快速生成一个集成了前端和后端的项目结构。

### 创建流程

1. **新项目初始化**  
   使用 Tauri 官方提供的命令创建新项目：

   ```bash
   pnpm create tauri-app --rc
   ```

   在此过程中，你会被引导选择前端框架（比如 Vue、React 或纯 HTML/JavaScript），工具会自动帮你生成基础项目。

   ![示例截图](https://img2024.cnblogs.com/blog/1078209/202409/1078209-20240913150546533-604436091.png)

2. **启动开发环境**  
   进入项目目录后，运行以下命令启动开发：

   ```bash
   pnpm tauri dev
   ```

   系统会自动构建桌面应用，并在 Windows 上以窗口显示。

   ![示例截图](https://img2024.cnblogs.com/blog/1078209/202409/1078209-20240913150616202-1703423301.png)

---

## 2. 将 Tauri 集成到已有的 Web 项目中

如果你已经有了一个 Web 项目（React、Vue等），想要快速转化为桌面应用，可以将 Tauri 集成到现有项目中，使其变成一个桌面版。

### 集成步骤

1. **进入现有项目目录**  
   确保你的项目已经是 Node.js 管理（即有 `package.json` 文件）。

2. **安装 Tauri 依赖**  
   执行：

   ```bash
   pnpm add -D @tauri-apps/cli@next
   ```

3. **初始化 Tauri 配置**  
   在项目中运行：

   ```bash
   npx tauri init
   ```

   系统会提示你配置应用信息（如应用名、窗口标题、前端资源路径等），根据提示填写即可。

   ![配置界面示意](https://img2024.cnblogs.com/blog/1078209/202409/1078209-20240913150628974-11842717.png)

   配置完成后，`src-tauri` 文件夹会生成，包含所有 Tauri 配置和 Rust 代码。

   ![目录结构示意](https://img2024.cnblogs.com/blog/1078209/202409/1078209-20240913150646448-1252870194.png)

4. **运行项目以进行调试**  
   使用命令：

   ```bash
   pnpm tauri dev
   ```

   你会看到你的 Web 项目在桌面窗口中运行，已经实现标准的与桌面交互。

   ![运行效果示意](https://img2024.cnblogs.com/blog/1078209/202409/1078209-20240913150658930-1026526704.png)

这样，你的现有 Web 应用就变成了一个桌面应用，能调用 Tauri 提供的丰富 API，实现对操作系统的访问。

---

## 小结

通过以上两种方式，无论是全新开发，还是在已有项目基础上集成，你都能轻松在 Windows 上快速启动 Tauri 应用。  
接下来，我们将深入学习 Tauri API，掌握如何利用 Rust 后端增强应用功能，实现更复杂的交互与功能扩展。









